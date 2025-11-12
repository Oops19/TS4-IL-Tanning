# Island Living - Tanning Fix 

A mod for Custom Tan Lines

This mod allows to apply custom tan lines for an outfit.
* It does not fix broken skin tones.
* It does not create a proper diffuse map (uv_0) for an existing, broken outfit.

If tanning works fine for your sims with all outfits this tool is likely not needed. I came across multiple UGC creators who UV unwrap their outfit mesh to random places on the diffuse map. While this may be good for layered outfits the effects on tanning are random. After tanning the sims have tan lines which don't match the outfit at all.

The same way to fix the broken outfits can be used to create tan-through outfits. Creating such outfits using good items is even more easy as one only needs to modify the opacity/transparency or remove some parts completely.

Creating custom tan lines
This part is quite long as it describes how to add custom tan lines. It does not describe how to use

Needful things
* An item to create the diffuse map for.
* The diffuse map to get an idea which parts to tan.
* Vanilla diffuse maps of various swimwear and similar outfits as a reference can be helpful.
* Gimp, Photoshop or any other image editor to create a new diffuse map.
* Sims4Studio, S4PE or any other tool to create a package file
* GUIDs
* Optionally Python to verify the 'data' *1)
* Optionally an XML editor to create the XML snippet *2

*1) Online: https://www.tutorialspoint.com/onli...n_formatter.htm - add 'a=' before the data. 'Beautify' checks the syntax and uglifies the code.
*2) Online: https://www.tutorialspoint.com/online_xml_editor.htm

The package file needs to contain:
* Diffuse map with the desired tan lines
* CAS Part Item referencing to the diffuse map.
* XML Snippet Tuning referencing the CAS Part Item and which CAS Part Items should be replaced.

Diffuse Map
The most easy way is to use a good diffuse map of an existing item and modify it. Make some areas transparent or increase the size of the opaque area.
Or merge 2-3 diffuse maps.

Naming and Instance ID
Select a name for your creation. I picked 'o19_yfBody_EF01SwimsuitOnePiece_TanThrough' as I modified a 'yfBody_EF01SwimsuitOnePiece_...' diffuse map.
I also named my diffuse map 'o19_yfBody_EF01SwimsuitOnePiece_TanThrough.png'

Use the hash generator to generate the fnv64 IDs (with High-Bit) in hex and dec:
Eg: 'o19_yfBody_EF01SwimsuitOnePiece_TanThrough' = (0x) ECA26728CACFC809 = 17051304564077086729
Write the name, hex and dec ID down as it is needed later.

One may either add this to the current package (for UGC) or create a new one for EA CAS Parts:
* Start S4S
* S4S > Tools > Create Empty Package

1) Image
* Add > RLE 2 Image > Group: 0, Instance: ECA26728CACFC809 (the written down hex ID)
* Select the image entry
* Import > File 'o19_yfBody_EF01SwimsuitOnePiece_TanThrough.png'

2) CAS Part
* Import or modify the CAS Part with the completely different instance key.
* Modify:
* Key.Instance to ECA26728CACFC809 [REF.hex]
* PartFlags.Allow*: Uncheck
* PartFlags.Default*: Uncheck
* PartFlags.Show*: Uncheck

3) Snippet Tuning
* Add > Snippet Tuning > Group: 0, Instance: ECA26728CACFC809 (matches the selected item) [REF]
* Select the Snippet Tuning entry
* Insert (n~[REF.name], s=[REF.dec]:
Code:
```xml
<>?xml version="1.0" encoding="utf-8"?>
<I c="TanningFix" i="snippet" m="tanning.snippet" n="o19_yfBody_EF01SwimsuitOnePiece_TanThrough" s="17051304564077086729">
  <;!-- ECA26728CACFC809 -->
  <T n="version"></T>
  <T n="data">
        {
            'BodyType.FULL_BODY': {
                0xECA26728CACFC809: [ 0x10927, 0x10928, 0x10929, 0x1092A, 0x1092B, 0x1092C, 0x1092D, 0x1092E, 0x1092F,
                    0x11929, 0x1192A, 0x1192B, 0x1192C, ],  # New swatches
                # Add the name behind the IDs, if it can be parsed it'll replace the line above
                'o19_yfBody_EF01SwimsuitOnePiece_TanThrough': ['yfBody_EF01SwimsuitOnePiece_*', ],
            },
        }
    </T>
</I>
```


Obviously obtaining the name and using a wildcard for the EA CAS parts is more easy than using the numbers. You may really want to start like this and check the log file. This mod searches all matching items and lists their IDs (as dec, these numbers match the hex numbers from above):
_get_int_cas_part_ids(o19_yfBody_EF01SwimsuitOnePiece_TanThrough) -> '{17051304564077086729}'
_get_int_cas_part_ids(yfBody_EF01SwimsuitOnePiece_*) -> '{67879, 67880, 67881, 67882, 67883, 67884, 67885, 67886, 67887, 71977, 71978, 71979, 71980}'


Save the package file.

Some more information
Instead of `'BodyType.FULL_BODY'` the number `5` may be used. The most commonly used types are:
Code:
```sh
HAT = 1
HAIR = 2
HEAD = 3
FULL_BODY = 5
UPPER_BODY = 6
LOWER_BODY = 7
SHOES = 8
NECKLACE = 12
GLOVES = 13
SOCKS = 36
TIGHTS = 42
```


Multiple settings can be joined, sections are separated with ','.
Another example of a dict which will obviously not work:
Code:
```json
{
    1: {
        12: [34, 56, ],
        11: [22, 33, ],
    },
    'BodyType.SOCKS ': {
        'foo': ['bar*', ],
        'kk': ['nik*', 'adi*', ],
    },
}
```

---

# 📝 Addendum

## 🔄 Game compatibility
This mod has been tested with `The Sims 4` 1.119.109, S4CL 3.17, TS4Lib 0.3.42.
It is expected to remain compatible with future releases of TS4, S4CL, and TS4Lib.

## 📦 Dependencies
Download the ZIP file - not the source code.
Required components:
* [This Mod](../../releases/latest)
* [TS4-Library](https://github.com/Oops19/TS4-Library/releases/latest)
* [S4CL](https://github.com/ColonolNutty/Sims4CommunityLibrary/releases/latest)
* [The Sims 4](https://www.ea.com/games/the-sims/the-sims-4)

If not already installed, download and install TS4 and the listed mods. All are available for free.

## 📥 Installation
* Locate the localized `The Sims 4` folder (it contains the `Mods` folder).
* Extract the ZIP file directly into this folder.

This will create:
* `Mods/_o19_/$mod_name.ts4script`
* `Mods/_o19_/$mod_name.package`
* `mod_data/$mod_name/*`
* `mod_documentation/$mod_name/*` (optional)
* `mod_sources/$mod_name/*` (optional)

Additional notes:
* CAS and Build/Buy UGC without scripts will create `Mods/o19/$mod_name.package`.
* A log file `mod_logs/$mod_name.txt` will be created once data is logged.
* You may safely delete `mod_documentation/` and `mod_sources/` folders if not needed.

### 📂 Manual Installation
If you prefer not to extract directly into `The Sims 4`, you can extract to a temporary location and copy files manually:
* Copy `mod_data/` contents to `The Sims 4/mod_data/` (usually required).
* `mod_documentation/` is for reference only — not required.
* `mod_sources/` is not needed to run the mod.
* `.ts4script` files can be placed in a folder inside `Mods/`, but storing them in `_o19_` is recommended for clarity.
* `.package` files can be placed in a anywhere inside `Mods/`.

## 🛠️ Troubleshooting
If installed correctly, no troubleshooting should be necessary.
For manual installs, verify the following:
* Does your localized `The Sims 4` folder exist? (e.g. localized to Die Sims 4, Les Sims 4, Los Sims 4, The Sims 4, ...)
  * Does it contain a `Mods/` folder?
    * Does Mods/_o19_/ contain:
      * `ts4lib.ts4script` and `ts4lib.package`?
      * `{mod_name}.ts4script` and/or `{mod_name}.package`
* Does `mod_data/` contain `{mod_name}/` with files?
* Does `mod_logs/` contain:
  * `Sims4CommunityLib_*_Messages.txt`?
  * `TS4-Library_*_Messages.txt`?
  * `{mod_name}_*_Messages.txt`?
* Are there any `last_exception.txt` or `last_exception*.txt` files in `The Sims 4`?


* When installed properly this is not necessary at all.
For manual installations check these things and make sure each question can be answered with 'yes'.
* Does 'The Sims 4' (localized to Die Sims 4, Les Sims 4, Los Sims 4, The Sims 4, ...) exist?
  * Does `The Sims 4` contain the folder `Mods`?
    * Does `Mods` contain the folder `_o19_`? 
      * Does `_19_` contain `ts4lib.ts4script` and `ts4lib.package` files?
      * Does `_19_` contain `{mod_name}.ts4script` and/or `{mod_name}.package` files?
  * Does `The Sims 4` contain the folder `mod_data`?
    * Does `mod_data` contain the folder `{mod_name}`?
      * Does `{mod_name}` contain files or folders?
  * Does `The Sims 4` contain the `mod_logs` ?
    * Does `mod_logs` contain the file `Sims4CommunityLib_*_Messages.txt`?
    * Does `mod_logs` contain the file `TS4-Library_*_Messages.txt`?
      * Is this the most recent version or can it be updated?
    * Does `mod_logs` contain the file `{mod_name}_*_Messages.txt`?
      * Is this the most recent version or can it be updated?
  * Doesn't `The Sims 4` contain the file(s) `last_exception.txt`  and/or `last_exception*.txt` ?
* Share the `The Sims 4/mod_logs/Sims4CommunityLib_*_Messages.txt` and `The Sims 4/mod_logs/{mod_name}_*_Messages.txt`  file.

If issues persist, share:
`mod_logs/Sims4CommunityLib_*_Messages.txt`
`mod_logs/{mod_name}_*_Messages.txt`

## 🕵️ Usage Tracking / Privacy
This mod does not send any data to external servers.
The code is open source, unobfuscated, and fully reviewable.

Note: Some log entries (especially warnings or errors) may include your local username if file paths are involved.
Share such logs with care.

## 🔗 External Links
[Sources](https://github.com/Oops19/)
[Support](https://discord.gg/d8X9aQ3jbm)
[Donations](https://www.patreon.com/o19)

## ⚖️ Copyright and License
* © 2020-2025 [Oops19](https://github.com/Oops19)
* `.package` files: [Electronic Arts TOS for UGC](https://tos.ea.com/legalapp/WEBTERMS/US/en/PC/)  
* All other content (unless otherwise noted): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 

You may use and adapt this mod and its code — even without owning The Sims 4.
Have fun extending or integrating it into your own mods!

Oops19 / o19 is not affiliated with or endorsed by Electronic Arts or its licensors.
Game content and materials © Electronic Arts Inc. and its licensors.
All trademarks are the property of their respective owners.

## 🧾 Terms of Service
* Do not place this mod behind a paywall.
* Avoid creating mods that break with every TS4 update.
* For simple tuning mods, consider using:
  * [Patch-XML](https://github.com/Oops19/TS4-PatchXML) 
  * [LiveXML](https://github.com/Oops19/TS4-LiveXML).
* To verify custom tuning structures, use:
  * [VanillaLogs](https://github.com/Oops19/TS4-VanillaLogs).

## 🗑️ Removing the Mod
Installing this mod creates files in several directories. To fully remove it, delete:
* `The Sims 4/Mods/_o19_/$mod_name.*`
* `The Sims 4/mod_data/_o19_/$mod_name/`
* `The Sims 4/mod_documentation/_o19_/$mod_name/`
* `The Sims 4/mod_sources/_o19_/$mod_name/`

To remove all of my mods, delete the following folders:
* `The Sims 4/Mods/_o19_/`
* `The Sims 4/mod_data/_o19_/`
* `The Sims 4/mod_documentation/_o19_/`
* `The Sims 4/mod_sources/_o19_/`
