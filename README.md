# # My Capstone: MTG Deck Wizard GPT

## Alex Wallace

## Description
Hello, everyone! This capstone project has ultimately been a journey to create a custom GPT that is able to help you create standard legal Magic: The Gathering (MTG) decks. The GPT is called 'MTG Deck Wizard' and the link to the GPT can be found [Here](https://chat.openai.com/g/g-0fvKEyecj-mtg-deck-wizard).

This GPT helps you build cohesive, standard decks from scratch using cards from the most recent 2024 standard rotation. It is really good at providing the user with recommendations on core cards from each set to center decks around, along with building decks using these cards and other supplemental cards from the legal standard sets. Although the GPT still has issues building full decks immediately from scratch (it sometimes reverts to its old standard knowledge when doing so), it is an extremely helpful tool to use when building decks step by step. 

In order to use this GPT, I recommend asking it to help you start building a deck of the chosen color/combo of colors you would like to use. Asking it for examples of cards in your specified colors from each legal set gives it a good starting point to start building a cohesive deck that is standard legal in 2024. You can then ask it to build a standard legal deck using a combo of these cards, and it will do so. It still has trouble creating effective mana bases using non-basic lands, but it can even be prompted to do that after creating the deck template. You can ask the GPT for all types of information for each card, the most common being the mana cost, set and quantity of the cards. Mess around with it and ask it to replace the occasional out-of-meta card, and you'll have a usable standard deck template that you can change at will just by asking the GPT to switch cards out or find lands that suit the mana base. 

## The Journey

There were a couple key hurdles regarding this project:
- ChatGPT 4 only has information from the internet up to April 2023, so it follows the previous standard rotation. As we are in March 2024, the standard MTG rotation is totally different. In MTG, the standard rotation changes each Fall, and consists of a different combination of sets that are legal to play. Since the landscape of standard was completely different in 2023 than it is now, I had to find a way of updating my GPT's idea of a standard legal deck to consist of the new standard rotation's sets, not the old ones. 
- ChatGPT 4 did not have references to cards from the most recent sets, such as 'Murders at Karlov Manor' (MKM), 'Lost Caverns of Ixalan' (LCI), and 'Wilds of Eldraine' (WOT) to name a few. Because of this, I needed to create a dataset with the information for the cards from these various sets. In order to do this, I set out on a quest to retrieve the card information from all sets legal in standard as of March 2024. 

At first, I got the idea to do this project by searching the internet for public APIs. I got the idea to do an MTG project after perusing a github repository of collected public APIs that can be found [Here](https://github.com/public-apis/public-apis). I found a public API called 'MTG-SDK' that allowed me to parse through cards and download the information for the cards I wanted.

After installing the MTG-SDK plugin in my virtual environment (venv), I started to download all the cards legal in the 2024 standard format from the API. My work parsing through this API can be found [Here](mtg_api.ipynb). At first, I thought I had gotten all the information I needed to create my standard format dataset, however, after realizing the set MKM had just come out, I found out that my dataset did not contain information from the most recent sets (MKM, LCI, and WOT). Because of this, the dataset I created was essentially obsolete for the purpose I had intended it for. 

After searching the internet for a possible solution, I ran into the website [mtgjson.com](mtgjson.com). I looked through the documentation of this website and found the CSV file [cards.csv](/data/cards.csv). I looked through this file and found that it had all MTG cards ever created, including the cards from MKM, LCI and WOT. This is exactly what I needed. 

After parsing through the sets of this CSV file, keeping the columns I was interested in, and organizing and cleaning the data, I created the [standard_cards.csv](/data/standard_cards.csv) dataset. This dataset included every card printed in each standard set, which includes cards from: 

- Innistrad: Midnight Hunt (MID)
- Innistrad: Crimson Vow (VOW)
- Kamigawa: Neon Dynasty (NEO)
- Streets of New Capenna (SNC)
- Dominaria United (DMU)
- The Brothers’ War (BRO)
- Phyrexia: All Will Be One (ONE)
- March of the Machine (MOM)
- March of the Machine: The Aftermath (MAT)
- Wilds of Eldraine (WOT)
- Lost Caverns of Ixalan (LCI)
- Murders at Karlov Manor (MKM)

Each card has its own unique name and uuid, along with the set each card comes from (as referenced by the three letter set codes included). The dataset also has all the necessary information needed to know what each card does and its specific type. Examples of columns with this information include the text, types, power, toughness, and mana cost of each card. Through this dataset, I figured that ChatGPT could use its knowledge of MTG to utilize this information and build decks using cards in this dataset. 

After creating the custom GPT 'MTG Deck Wizard' (I will show how to do this in the setup portion of this README), I approached feeding this dataset to the GPT in a couple of different ways. At first, I wanted to create an API and an OPEN API specification file so I could use the API as an action within the GPT. I still plan on doing this, as it seems to be the most accurate way to get the GPT to act how I'd like it to. However, given the size of the dataset wasn't too large, I opted to try and feed the GPT the CSV file through the code interpreter, which ended up being very successful after I had prompted the GPT to act the way I wanted it to and use the CSV file properly. I will also explain in more detail how I did this in the setup portion of this README. 

While the GPT still has issues at times with building full decks immediately from scratch, I've found that it can successfully query my dataset a large majority of the time, and if you try and build the deck in steps rather than all at once, it does an amazing job of utilizing the information for the new cards along with its previous knowledge of MTG decks. I will provide a couple of example images on how well it has worked for me. 

![Image](/images/izzet_deck.png)

As you can see in this image, the GPT was able to give me a full 60 card deck recommendation fully equipped with some of the strongest cards in standard, along with a decent example mana base I could go off of. It was even able to name the deck and offer potential sideboard cards from the dataset. The whole conversation I had with my GPT in this image can be found at the link [Here](https://chat.openai.com/share/e/43c658f5-e704-4b6d-8a87-8f9786d7c951). If you open this chat, you can see some minor issues that the GPT had when querying the dataset, but it ends up correcting itself 99% of the time. It also has some slight issues with finding non-basic lands in the dataset, but I created the [standard_mana.csv] to help it find those lands. It just needs an extra prompt at times to do this task. 

Overall, I am very pleased with the performance of this GPT up until this point. Despite having some issues and bugs, the user can make cool themed decks to use in standard play, and can even choose specific cards to build the deck around. The GPT is also able to tell the user what each card in the new dataset does just by finding the information within the CSV file. 

I will be updating this GPT as soon as the information for the cards of each new set can be found online. I will also update the rotation of sets it uses to make standard decks as soon as the rotation changes in Fall of 2024. I have some ideas on how to automate this process, and I will explain how I aim to do this in the 'improvements' section of this README. Please feel free to contact me if you have any suggestions on how to improve this GPT, or if you would like to work on code to help improve its capabilities. 

## Setup/Installation Requirements
If you would like to experiment with the 'MTG-SDK' API, you will need to make a directory for your file and then switch over to that directory. Then, create a virtual environment for python 3 to work in. Change into your virtual environment using source venv/bin/activate. You will need to install the requirements.txt file

```
pip install -r requirements.txt
```

After doing this, you will be able to import the mtgsdk module. For more information on how to use this API, follow the link [Here](https://github.com/MagicTheGathering/mtg-sdk-python/blob/master/README.md)

If you would like to download the files from mtgjson.com, got to the link [Here](mtgjson.com). If you would like to download the CSV files already cleaned, I created a Kaggle dataset that you can download [Here](https://www.kaggle.com/datasets/hlfkrnzombie/mtg-standard-datasets/data). Through these options, you should be able to find whatever information you need regarding any MTG card ever printed. 

In order to create a custom GPT, you need to have a ChatGPT-4 subscription activated. Go to [openai.com](openai.com) to create an account and activate your subscription. After doing this, navigate to chatGPT and click on 'explore GPTs' located on the left sidebar. After doing this, click the '+ create' button in the top right corner. This will take you to the custom GPT creation UI.

![Image](/images/create_gpt.png)

In order to configure your GPT, press the 'configure' button on the left side of the UI. It will take you to a screen that looks like this:

![Image](/images/configure_gpt.png)

On this page, you will be able to name your GPT, give a description of what it does, give it instructions on what its supposed to do and how its supposed to talk, and upload any knowledge files you want it to use when looking for information to respond to user prompts. For this project, I uploaded a couple of CSV files for the GPT to query when searching for information on new cards. I also uploaded an experimental jupyter notebook to try and help the GPT produce queries, but it ended up confusing the GPT more so I told the GPT not to use it.

After uploading these files, I gave the GPT a bunch of prompts to learn from, and tested the GPT on the right side of the UI. This took a lot of trial and error. I found that concise, direct messages telling the GPT what to do worked the best. I fed the GPT over 150 learning prompts, the most important of which I'll paraphrase:
- When building standard legal decks, use the updated 2024 sets: MID, VOW, NEO, SNC, DMU, BRO, ONE, MOM, MAT, WOT, LCI, MKM
- Do not add any of the banned cards to standard decks: 'The Meathook Massacre', 'Fable of the Mirror Breaker', 'Invoke Despair', and 'Reckoner Bankbuster'
- Consult the 'standard_cards.csv' for all cards present in the legal standard sets
- Consult the 'standard_lands.csv' for all nonbasic lands present in the legal standard sets (it had the most difficulty finding proper nonbasic lands to use, so I created another CSV file to make it easier for the GPT)
- Use these column names when querying dataframe: uuid,number,name,setCode,text,manaCost,manaValue,colors,colorIdentity,power,toughness,type,types,subtypes,supertypes,rarity,artist,printings,language (this one was important as it vastly increased query success within the GPT).
- Please offer a mix of cards from all 10 legal standard sets when building standard legal decks. 

Using prompts like this slowly enabled the GPT to replace its previous knowledge of the MTG standard rotation to the new 2024 standard rotation. The GPT would still bug out and recommend cards from outdated datasets, but did this less and less the more I trained it to use the 'standard_cards.csv' dataset. 

Once you're ready to publish your GPT, click the teal 'publish' button in the upper right hand corner. This will give you a couple of options on who to publish it to. I went ahead and published it to the public, but you could choose to make it more private if you'd like. 

If you ever need to edit your custom GPT, navigate to the GPT on your left sidebar, and click on the GPT name in the top of the GPT conversation area. This will incite a dropdown menu where you can click 'edit GPT' to give it more guidance on what to do. 

Using methods similar to the ones i've outlined above, you can create your own custom GPT to do pretty much anything you want, and to read datasets and respond with information in said datasets. It takes some patience, as the GPT can act somewhat robotic at times, but with perseverance, you'll start forming your GPT to act exactly how you want it to regardless of what the user inputs.

## Improvements 
Will be updated after the presentation.

## Known Bugs
Will be updated after the presentation.

## License
Copyright 2023 Alex Wallace

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.