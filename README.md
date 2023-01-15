# GDScript Mod Loader

A general purpose mod-loader for GDScript-based Godot Games.

For detailed info, see the [docs for Delta-V Modding](https://gitlab.com/Delta-V-Modding/Mods/-/blob/main/MODDING.md), upon which ModLoader is based. The docs there cover mod setup and helper functions in much greater detail.

## Mod Setup

### Structure

Mod ZIPs should have the structure shown below. The name of the ZIP is arbitrary.

```
yourmod.zip
├───.import
└───mods-unpacked
    └───Author-ModName
        ├───mod_main.gd
        └───manifest.json
```

#### Notes on .import

Adding the .import directory is only needed when your mod adds content such as PNGs and sound files. In these cases, your mod's .import folder should **only** include your custom assets, and should not include any vanilla files.

You can copy your custom assets from your project's .import directory. They can be easily identified by sorting by date. To clean up unused files, it's helpful to delete everything in .import that's not vanilla, then run the game again, which will re-create only the files that are actually used.


### Required Files

Mods you create must have the following 2 files:

- **mod_main.gd** - The init file for your mod.
- **manifest.json** - Meta data for your mod (see below).

#### Example manifest.json

```json
{
    "name": "ModName",
    "version": "1.0.0",
    "description": "Mod description goes here",
    "website_url": "https://github.com/example/repo",
    "dependencies": [
		"Add IDs of other mods here, if your mod needs them to work"
    ],
    "extra": {
        "godot": {
            "id": "AuthorName-ModName",
            "incompatibilities": [
				"Add IDs of other mods here, if your mod conflicts with them"
			],
            "authors": ["AuthorName"],
            "compatible_game_version": ["0.6.1.6"],
        }
    }
}
```

#### Notes on meta.json

Some properties in the JSON are not checked in the code, and are only used for reference by yourself and your mod's users. These are:

- `version`
- `compatible_game_version`
- `authors`
- `description`
- `website_url`


## Helper Methods

Use these when creating your mods. As above, see the [docs for Delta-V Modding](https://gitlab.com/Delta-V-Modding/Mods/-/blob/main/MODDING.md) for more details.

### install_script_extension

	func install_script_extension(child_script_path:String)

Add a script that extends a vanilla script. `child_script_path` is the path to your mod's extender script path, eg `MOD/extensions/singletons/utils.gd`.

Inside that extender script, it should include `extends {target}`, where {target} is the vanilla path, eg: `extends "res://singletons/utils.gd"`.

Your extender scripts don't have to follow the same directory path as the vanilla file, but it's good practice to do so.

One approach to organising your extender scripts is to put them in a dedicated folder named "extensions", eg:

```
yourmod.zip
├───.import
└───mods-unpacked
    └───Author-ModName
        ├───mod_main.gd
        ├───manifest.json
        └───extensions
            └───Any files that extend vanilla code can go here, eg:
            ├───main.gd
            └───singletons
                ├───item_service.gd
                └───debug_service.gd
```

### add_translation_from_resource

	add_translation_from_resource(resource_path: String)

Add a translation file, eg "mytranslation.en.translation". The translation file should have been created in Godot already: When you import a CSV, such a file will be created for you.

Note that this function is exclusive to ModLoader, and departs from Delta-V's two functions [addTranslationsFromCSV](https://gitlab.com/Delta-V-Modding/Mods/-/blob/main/MODDING.md#addtranslationsfromcsv) and [addTranslationsFromJSON](https://gitlab.com/Delta-V-Modding/Mods/-/blob/main/MODDING.md#addtranslationsfromjson), which aren't available in ModLoader.

### append_node_in_scene

	append_node_in_scene(modified_scene, node_name:String = "", node_parent = null, instance_path:String = "", is_visible:bool = true)

Create and add a node to a instanced scene.

### save_scene

	save_scene(modified_scene, scenePath:String)

Save the scene as a PackedScene, overwriting Godot's cache if needed.


## Credits

🔥 ModLoader is based on the work of these brilliant people 🔥

- [Delta-V-Modding](https://gitlab.com/Delta-V-Modding/Mods)
