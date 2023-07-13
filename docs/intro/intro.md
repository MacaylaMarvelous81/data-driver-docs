Title:   Introduction to Data Driver Docs
Summary: Guide to help people learn how to use Data Driver.

### What is Data Driver?
Data Driver is a library mod for
[Crypt of the Necrodancer: Synchrony](https://braceyourselfgames.com/crypt-of-the-necrodancer)
which lets mod creators make items through data files. Only JSON files and
item entities are supported currently, but more file types and entities are
in the works.

### Adding a Dependency
!!! note
	This guide will not go over how to create Synchrony mods, but only on how
	to use Data Driver. To learn about Synchrony modding, look at the
	[Synchrony Modding Documentation](https://vortexbuffer.com/synchrony/docs).

Before using Data Driver in your mod, you should first mark it as a dependency
in your `mod.json` file in the `dependencies` section. Your mod file should
look similar to this:
```json title="mod.json" hl_lines="8-10" linenums="1"
{
	"namespace": "ExampleMod",
	"displayName": "Example Mod",
	"version": "1.0.0",
	"synchronyVersion": "3.7.1",
	"description": "No description given.",
	"author": "MacaylaMarvelous81",
	"dependencies": {
		"DataDriver": "1.0.0" // (1)!
	},
	"api": {"scriptPath":""},
	"icon": "ExampleModIcon.png",
	"banner": "ExampleModBanner.png",
	"tags": [],
	"name": "ExampleMod"
}
```

1. The value is the version of Data Driver your mod is dependent on. Be sure to
   check that you're specifying the same version of the mod that you're using.

Now you're ready to start using Data Driver!

### Defining an Item
To make an item, create a new JSON file in the `data/items` directory of your
mod. This file will represent the definition of your item. Item definitions are
made up of a few fields:

- name: The item's unique identifier.
- hint: The item's hint, which describes the item and appears above the item.
- slot: The slot the item should take.
- components: The components which make up the item.

??? example
	The item definition for a helmet item might look like this:

	=== "Item Definition"

		```json title="cosmic_helm.json" linenums="1"
		{
			"name": "CosmicHelm",
			"hint": "Cosmetic item",
			"slot": "head",
			"components": {
				"friendlyName": {
					"name": "Cosmic Helm"
				},
				"sprite": {
					"texture": "mods/ExampleMod/sprites/cosmic_helm.png"
				}
			}
		}
		```

	=== "Lua Equivalent"
	
		```lua title="cosmic_helm.lua" linenums="1"
		local customEntities = require "necro.game.data.CustomEntities"

		customEntities.extend({
			name = "ExampleModCosmicHelm",
			template = customEntities.template.item(),
			data = {
				hint = "Cosmetic item",
				slot = "head"
			},
			components = {
				friendlyName = {
					name = "Cosmic Helm"
				},
				sprite = {
					texture = "mods/ExampleMod/sprites/cosmic_helm.png"
				}
			}
		})
		```

!!! bug annotate "Gotcha!"
	Since Data Driver is the mod which creates the item, Crypt of the
	Necrodancer will use its namespace instead of the namespace of your mod.
	Furthermore, Data Driver will prefix the name of the item with your mod's
	**folder name** to keep the item names unique across mods. This may
	result in the item's name being different from what you may expect.

	!!! example
		If a mod located in the folder "ExampleMod" creates an item with the
		name "CosmicHelm", the full name will end up being
		"DataDriver_ExampleModCosmicHelm".

Now the item will be registered whenever Data Driver is loaded, so you can use
it in your mod!
