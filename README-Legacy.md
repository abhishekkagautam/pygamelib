![Linux: Ok](https://img.shields.io/badge/Linux-Ok-green.svg "Linux: Ok")
![Windows: Ok](https://img.shields.io/badge/Windows-Ok-green.svg "Windows: Ok")
![Mac OS: Ok](https://img.shields.io/badge/Mac%20OS-Ok-green.svg "Mac OS: Ok")
[![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.txt)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Documentation Status](https://readthedocs.org/projects/hac-game-lib/badge/?version=latest)](https://hac-game-lib.readthedocs.io/en/latest/?badge=latest)
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/2849/badge)](https://bestpractices.coreinfrastructure.org/projects/2849)
[![CircleCI](https://circleci.com/gh/arnauddupuis/hac-game-lib.svg?style=svg)](https://circleci.com/gh/arnauddupuis/hac-game-lib)

# hac-game-lib
Hyrule Astronomy Club Game Library- base library for a game development

## Name change

With the growing popularity of the library we decided that it could not longer be named after our little science club.

So we took the decision to rename it to [**pygamelib**](https://pypi.org/project/pygamelib/). In the process the whole file/class hierarchy as been reworked.

Therefore the pygamelib 1.2.0+ is ***not*** backward compatible with the hac-game-lib. We are talking about **name compatibility**, 
the conversion to the new library is very easy (please see bellow for a guide). Everything else is 100% compatible (it's the same library coded by the same people).

## How to upgrade to pygamelib

Simply upgrade using pip:
```bash
pip3 install --upgrade --user hac-game-lib
```

This will automatically install pygamelib and obsoletes the hac-game-lib module. 

If you want to install system wide, not just for your current user, remove the --user option from the command line. 

## Convert from the hac-game-lib to pygamelib

The files and directories naming are now more aligned with [PEP 8](http://www.python.org/dev/peps/pep-0008/#package-and-module-names) and [PEP 423](https://www.python.org/dev/peps/pep-0423/). 
We used the fact that renaming and restructuring was going to break everything to reduce the proliferation of modules with just one class. We rationalized a bit.

So without further ado:

 * gamelib.Game, gamelib.Board, gamelib.Inventory are now unified into **pygamelib.engine**.
 * gamelib.HacExceptions and gamelib.Utils are now unified into **pygamelib.base**. For exceptions, Hac prefix was replaced by Pgl but for convenience mirror classes were added to not break existing games.
 * gamelib.BoardItems, gamelib.Movable, gamelib.Immovable, gamelib.Characters and gamelib.Structures are now unified into **pygamelib.board_items**.
 * gamelib.Actuators.Actuator, gamelib.Actuators.SimpleActuators, gamelib.Actuators.AdvancedActuators are now unified into **pygamelib.actuators**.
 * gamelib.Sprites was deprecated in version 1.1.0 in favor of gamelib.Assets.Graphics.Sprites and is now removed (please continue reading).
 * gamelib.Assets is now **pygamelib.assets**.
 * gamelib.Assets.Graphics is now **pygamelib.assets.graphics**.
 * gamelib.Assets.Graphics.Sprites has been renamed to **pygamelib.assets.graphics.Models**.

The gamelib.Utils module is the one that is probably going to require the most attention. It is being exploded and removed.

 * The colored squares and rectangle are now in pygamelib.assets.graphics with the exact same name.
 * The coloring text functions have been moved to a new Text class in pygamelib.base.
 * the get_key() function was moved to **pygamelib.engine.Game**.

There is also new modules and features but please see the release notes of [pygamelib](https://pypi.org/project/pygamelib/).

The general idea is to limit the number of imports and to group things by functional similitude.

Hopefully it's not too much work to convert your software.

## Real breaking changes

There is a couple of things that changed and are breaking previous implementations:

 * BoardItem.size() -> BoardItem.inventory_space(): with the introduction of pygamelib.gfx.core.Sprite and BoardComplexItem we needed to have actual item's size. Previously size was used to calculate how much space was used in the inventory. The internals are up to date but if you were using the size attribute, you should pay attention to that change.
 * Projectile: *args replaced by callback_parameters. In the Projectile class, the extra parameters passed to the hit_callback were a mess. So it is now using the same formalism than the GenericActionableStructure.

There is also a lot of class variables that were changed into properties. Please read the documentation.
In case it was not clear Sprites (pygamelib.gfx.core.Sprite) are not describing the same thing as before (formerly pygamelib.Assets.Sprites now pygamelib.assets.graphics.Models).