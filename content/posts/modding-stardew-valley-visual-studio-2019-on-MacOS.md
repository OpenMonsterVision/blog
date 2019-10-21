---
title: "Modding Stardew Valley using Visual Studio 2019 On MacOS"
date: 2019-10-20T17:45:48-06:00
tags : ["stardew valley", "sdv", "modding", "games", "mac", "visual studio"]
draft: false
---
<link rel="stylesheet" href="https://openmonstervision.github.io/blog/css/prism.css" />
<script src="https://openmonstervision.github.io/blog/js/prism.js" type="text/javascript"></script>
# The best modding information

The greatest resource for modding is of course, the [wiki](https://stardewvalleywiki.com/Modding:Player_Guide/Getting_Started). For me though it did have an outdated bit.

# Setting up your project

You'll have to set your project up under Other .NET, And Library. The Wiki was outdated and didn't have good screenshots for this.

![.NET](https://openmonstervision.github.io/blog/images/one.png)
![Library](https://openmonstervision.github.io/blog/images/two.png)


# xnb files
You should get familiar with the various XNB files that make modding the game very easy.
<br />
[dialog](https://stardewvalleywiki.com/Modding:Dialogue)
<br />
[schedule](https://stardewvalleywiki.com/Modding:Schedule_data)
<br />
[General NPC Data](https://stardewvalleywiki.com/Modding:NPC_data)

# Adding a new character

I would only use a mod like this on new saves. I have had some issues, with removing the mod afterword, and then the savegame is no good. I think this can be remedied but have not looked into it.
Here is the ModEntry.cs file:
``` csharp

using System;
using System.Collections.Generic;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using StardewModdingAPI;
using StardewModdingAPI.Events;
using StardewValley;


namespace Mary
{
    public class ModEntry : Mod, IAssetEditor, IAssetLoader
    {
        private IModHelper modHelper;
        public override void Entry(IModHelper helper)
        {
            modHelper = helper;

            helper.Events.GameLoop.GameLaunched += this.OnGameLaunched;
        }

        public bool CanEdit<T>(IAssetInfo asset)
        {
            // Prepering to edit files.
            if (asset.AssetNameEquals("Data/NPCDispositions"))
                return true;

            if (asset.AssetNameEquals("Data/NPCGiftTastes"))
                return true;

            if (asset.AssetNameEquals("Characters/Dialogue/rainy"))
                return true;

            if(asset.AssetNameEquals("Characters/Dialogue/rainy"))
                return true;

            return false;
        }

        public void Edit<T>(IAssetData asset)
        {
            // Editing existing game files.
            if (asset.AssetNameEquals("Data/NPCDispositions"))
            {
                asset.AsDictionary<string, string>().Data["Mary"] = "adult/shy/outgoing/positive/female/datable/null/Town/fall 9//Tent 2 3/Mary";
            }

            if (asset.AssetNameEquals("Data/NPCGiftTastes"))
            {
                IDictionary<string, string> NPCGiftTastes = asset.AsDictionary<string, string>().Data;
                NPCGiftTastes["Mary"] = "Now that is a lovely gift, @. It will really help me!/227 228 220 428 440" +
                    " 787 247/That's so nice of you, @!/724 725" +
                    " 726 303 350/Uh... I'm not sure what should I do with this./2 24 90 174 190 336" +
                    " 194/Why would you even give this to me?/149 205 281 404 420 422/Thank you.//";
            }

            if (asset.AssetNameEquals("Characters/Dialogue/rainy"))
            {
                IDictionary<string, string> rainy = asset.AsDictionary<string, string>().Data;
                rainy["Mary"] = "I hate it when it rains.$s";
            }
        }

        public bool CanLoad<T>(IAssetInfo asset)
        {
            //Preparing to load the assets.
            if (asset.AssetNameEquals("Characters/Dialogue/Mary"))
            {
                return true;
            }

            if (asset.AssetNameEquals("Characters/Mary"))
            {
                return true;
            }

            if (asset.AssetNameEquals("Portraits/Mary"))
            {
                return true;
            }


            if (asset.AssetNameEquals("Characters/schedules/Mary"))
            {
                return true;
            }


            return false;
        }

        public T Load<T>(IAssetInfo asset)
        {
         

            if (asset.AssetNameEquals("Characters/Dialogue/Mary"))
            {
                return Helper.Content.Load<T>("assets/Mary_dialogue.xnb", ContentSource.ModFolder);
            }

            if (asset.AssetNameEquals("Characters/schedules/Mary"))
            {
                return Helper.Content.Load<T>("assets/Mary_schedule.xnb", ContentSource.ModFolder);
            }

            if (asset.AssetNameEquals("Characters/Mary"))
            {
                return Helper.Content.Load<T>("assets/texture.png", ContentSource.ModFolder);
            }

            if (asset.AssetNameEquals("Portraits/Mary"))
            {
                return Helper.Content.Load<T>("assets/portrait.png", ContentSource.ModFolder);
            }


            throw new InvalidOperationException($"Unexpected asset '{asset.AssetName}'.");
        }




        private void OnGameLaunched(object sender, GameLaunchedEventArgs e)
        {


            Monitor.Log($"Mary Mod Loading");

        }


    }
}
```

As you can see in the code I did have to create a schedule file and a dialogue file, which I created by unpacking a different characters xnbfiles, using [xnbcli](https://github.com/LeonBlade/xnbcli). The xnbfiles of varying types can be found where ever your steam library is: SteamLibrary/steamapps/common/Stardew Valley/Contents/Resources/Content/. It's very easy to add filesm as you can see in the Load function. Sometimes your files won't load for various reasons, it's going to be a little trial and error, I had some issues with the schedule ones. Just keep fiddling. 

## Currently Unemployed

As I am currently unemployed perhaps you would want to click my amazon affiliate link and buy a [C# Book on Game Programming](https://www.amazon.com/gp/product/0134659864/ref=as_li_qf_asin_il_tl?ie=UTF8&tag=bsdpblog-20&creative=9325&linkCode=as2&creativeASIN=0134659864&linkId=7ab0cb6cfa0116eee2d1f1c12caddab9)


