# Porting of `SamSWAT-SCARH`
![alt text](https://media.discordapp.net/attachments/417281262085210112/863173813407580240/scar-h_tex_overhaul_comparison.png)
![alt text](https://media.discordapp.net/attachments/417281262085210112/863173800660172880/scar-h_fde_tex_overhaul_comparison.png)
\
This documentation is meant as an easy-to-follow tool to help the reader understand what needs to be done during the `porting` process. 

\
I, and anyone has permission to do this.
\

**Here are my papers**
![permission slip](https://cdn.discordapp.com/attachments/811997332988100628/905496544034316318/unknown.png)
If something in this tutorial is unclear then I need a proper description of what is unclear so that it can be updated.
\
\
`I am not responsible for what happens with this, and I also do not care.`
## Pre-requisites

 - [UABE - Unity Asset Bundle Extractor](https://github.com/DerPopo/UABE/releases)
 - [VSCodium (or Alternative)](https://vscodium.com/#install)
 - [Sublime Text (or Notepad)](https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)


## `What You're Looking At`

`... and what is actually important.`

![1](https://i.imgur.com/8mu6Ljm.png)

The only folders you need to have are the above highlighted, don't go and delete the others because I'm saying this but you only need to pay attention to those.\

### `What are these files/folders?`
`bundles` - has the assets or `.bundles` we need to modify to work in-game\
`db` - has the information we need to get the item in-game\
`bundles.json` - has information we need to modify the `.bundles` and `dump.txt` properly 



# `Getting Set-Up To Bundle Edit`

- Open `bundles.json` with **VSCodium**
This has the information we're going to need to get our converted assets to link up properly and work in-game.\
We will be using `bundles.json` to correct the `dumps` for each individual `.bundle` as well as creating `.manifests`


![1](https://i.imgur.com/dFcN7rw.png)
\
It probably looks a little daunting but everything will make sense in a bit.
### `What You're Looking At`
 `"key"` - the internal path of the `.bundle`
 - Consider `"key"` to be the bundles real name and the filename as its nickname
 `"dependencyKeys"` - the **link** names of the bundle

\
All of these names to be exact - and it's safe practice to make `.bundle.manifest`\
We're going to be focusing on the `.container` bundle, because if you understand how to properly configure that then all the other parts are relatively simple.

### `What I Like To Do`
The workflow that I have come up with starts with cleaning up the `bundles.json` so we can easily copy & paste for the `.manifest` and the `dump`\
To do that I erase everything that is unneeded:

![select](https://i.imgur.com/edRvlck.png)

- Hit `CTRL+F`, hit the drop-down arrow, leave *Replace* empty and hit `Replace All`

![selected](https://i.imgur.com/uQ96S5O.png)

... and it should look how it does below. 

![what](https://i.imgur.com/nxcDMJN.png)

**Repeat** the steps for the below:

![](https://i.imgur.com/Q68ta6w.png) 

![](https://i.imgur.com/Mfwzv8a.png)

### `Final Result of Cleanup`
![](https://i.imgur.com/CFcePea.png)

What we did is use some quick shortcuts to save time from having to go to each line and delete these unneeded characters and spaces!
\
Save and minimize this file so we can come back to it!

# `UABE and Bundle Editing`
- `File`
  - `Open`
    - `.\SamSWAT-SCARH\bundles\assets\content`
![](https://i.imgur.com/8AUWmDm.png)

  This is what you'll be staring at. \
  `What do we do?` \
  **Well, we're gonna choose a folder and start going.**\
  You'll hopefully come up with your own flow for things as we progress and you get practice in. You'll start to learn that there is sort of a pyramid effect and be able to tell what needs to be done first.
### `Fixing Audio Bundles`
I'm in now in `.\audio\banks`, full path is `.\SamSWAT-SCARH\bundles\assets\content\audio\banks`.

![](https://i.imgur.com/kikTTwR.png)

- Create a folder named `ported_banks` (we're going to be saving our fixed bundle here)
- Delete `mk17.bundle.manifest` (We'll be making a proper one later)
- Select the `mk17.bundle`
  - Copy the File name (we're gonna need it later)
- Open

- Click `Info` button and be brought to the `Assets info` window

![](https://i.imgur.com/AQTsggF.png)

- Click the `Container` tab and find and select `Type` **AssetBundle**
![](https://i.imgur.com/bqgmlHT.png)

- Click `Export Dump`
- Paste the `File name` you saved earlier, erase the `.bundle` extension
- Save, it will bring you back to `Assets info`
- Click `Import Dump`
- Right-click `mk17.txt` you just created and select `Open`
![](https://i.imgur.com/bK5Jgy5.png)

- Re-open your `bundles.json` 
  - We know that we're in `\audio\banks`, so CTRL+F `banks`
  - Find the corresponding `key`

![](https://i.imgur.com/nUd4R5T.png)

As stated earlier, `key` is the true name of this file.\
**Everything in these steps needs to be exact**


 - Copy `assets/content/audio/banks/mk17.bundle` and return to our dump
 - `string m_Name` and `string m_AssetBundleName` need to match the `key`, 
    - Paste over whatever is in those quotes even if it's the same because we want to make sure
 - `m_Dependencies` **needs to be exact** to the `dependencyKeys` in `bundles.json`

It looks as if `\audio\banks` is in order, so we are going to save the `dump`.
- You shouldn't had closed the `Import Dump` window, if you have, `Import Dump` and select your edited dump
- Select `OK` at the bottom and you should be prompted to `Would you like to save the changes?`
  - Select `Yes`
  - For workflow purposes, we're going to press `File -> Open` and be prompted to save again, hit `Yes`
  - Select the bundle you're in (mk17.bundle) so that it fills out the File name
  - Open `ported_banks` and save the bundle

`We need to create a manifest to supplement the bundle`
- Go into your `dump` file
- **Erase the contents** *(we're going to use this as staging)*
- Go over to your minimized `bundles.json` and copy the `dependencyKeys`

![](https://i.imgur.com/bJxlXBw.png)
- Paste into your `dump` that you erased
  - Remove any spaces at the end of any of the lines (or you will get an error)
- `Save As...`
  - Your manifest will be the bundle name plus `.manifest`
    - `mk17.bundle.manifest`
    
![](https://i.imgur.com/KKW4FSL.png)

This is what your `ported_banks` folder will resemble. You're probably thinking that it was pretty simple, and realistically it is.\
We do need to do this for every bundle, and making sure each bundle links up correctly is the only thing we're doing. \
\
Everything I have covered above **needs to be followed exactly for every bundle** and is repeated from here on out

Continuing, I have moved to `.\SamSWAT-SCARH\bundles\assets\content\audio\weapons` and am, `mk17` and it is asking me something that wasn't covered:

![](https://i.imgur.com/tstwLwa.png)
- Select `Yes`
  - You have been brought to a `Save As` window, name the file `1` (it really doesn't matter)
  - Save
  - Continue with the above steps

### `Fixing The Weapon`
![](https://i.imgur.com/i84kTyR.png)

We're now in `\SamSWAT-SCARH\bundles\assets\content\weapons\scar-h`, and this is what your folder should look like. For the moment we're going to ignore `tan`.
\
\
**The order in which to tackle this is as follows:**
1. `textures`
  * `textures bundle` - bundle with all textures
  * `materials bundle` - bundle with meshes
2. `client_assets.bundle` - bundle that links the meshes & textures
3. `animator` - bundle with weapon animations (not always present)
4. `_container` - bundle that makes up all links all below bundles to create the `weapon`
  * `tan` is also a `_container` and can be done last as well (not always present)


\
With all the bundles done (if done correctly), all you would need to do is create the databases for them.
I will eventually get around to *maybe* making a tutorial for that but there are plenty of templates that can be used and too many versions for me to be bothered at the moment.
## Feedback

If you have any feedback, please reach out to myself on the Altered Escape discord @King in the #modification-chat

