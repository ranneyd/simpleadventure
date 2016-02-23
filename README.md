# Simple Adventure
Simple Adventure allows you to make a simple story and art based adventure game. You create a configuration file, provide the art, run this app and get your game.

## What kind of a game does this make?
Simple Adventure makes a really simple game. The game will be a series of rooms with objects in them. When the user clicks on an object, something happens. This something can either be a transition to another scene, initiation of a conversation, or the setting of a variable. These should be the only options necessary to make a simple interactive game

For instance, you can have a simple haunted house. You begin in a room with some spooky objects like a diary and a lever and a door. The user can click on the diary and the game starts a "conversation" with the diary. The conversation just consists of the "diary" saying one thing, which is whatever you want the player to know about the diary, and the player can have one option to respond, which is "leave". The user could click on the level, which could set a variable called "leverPulled" to true, and somewhere down the line, you can do something with that variable. The user could click on the door, and it could transition them to a different 'scene', which is really just a room. You provide the art and the rules and the game is made for you.

## How do I do it? 

Check out [this tutorial](tutorial.md).

### Step 1: Get Art
Collect a bunch of image files to use in your game. You are going to need backgrounds, or "scenes", and characters, or "objects". 

Things to keep in mind:
 - You will need to provide things like doors or arrows for transitioning to different scenes.
 - Your scenes should all be the same size and shape. You will specify the height and width of your game. If your scenes aren't all the same size, it's going to look super weird.
 - Your files can be `.jpg`, `.png`, `.gif`, `.bmp`, or `.svg`. You CAN mix and match (you can have a few jpgs, a gif, an svg, and the rest pngs)
 - Your objects do NOT need to be the size you want them to be in game. You can specify what size you want them to be in the configuration file. However, the actual size of the image is its default value, so if you do not specify dimensions in the configuration file, the normal size will be used.

### Step 2: Set up your folder
Make a folder in the same place as this app (aka your `maker.js` file). Name the folder whatever you want the file for your game to be called. For our haunted house we could make it "haunted_house". Note that this is not the "name of your game", this is just for reference purposes, so keep it simple and make sure it's a legal file/folder name (remember, Microsoft Word is just "word.exe", Adobe Photoshop Pro CS6 is just "photoshop.exe"). Inside this folder, make two folders: "objects" and "scenes". Then, in the same folder make a file called "make.json". 

Recap: You have a folder called `your_game`. Inside are three items, two folders and a file, called `objects`, `scenes`, and `make.json`. 

Put all the scenes you got from Step 1 in the "scenes" folder and all the objects in the "objects" folder.

### Step 3: Make your configuration file
Open up `make.json` in some text editor like [Sublime Text](https://www.sublimetext.com/). This is where you define all the rules for your game. 

If you do not know what "JSON" means, scroll down to "I don't know what JSON means"

Your file MUST follow the following rules:

 - Your file is one big object. Inside that object are the following:
    - `title` STRING: what we're going to show on the start page.
    - `height` STRING: the height of your game in pixels. All your scene images should be this height.
        - PROTIP: for numbers, you don't actually need the quotes.
    - `width`STRING:  the width of your game. All your scene images should be this width.
    - `scenes` LIST: contain only OBJECTS, each representing a scene in your game
- Scene objects look like this
    - `id` STRING: a UNIQUE identifier that you will use to reference your scenes.
    - `type` STRING: this is `"location"` if this is a location, or `"convo"` if this is a conversation
    - If it's a `location`:
        - `img` STRING: this is the name of the image that will be used as the background in this scene. We will look in your `scenes` folder for a file with this name. 
        - `text` STRING OPTIONAL: This is text that will appear at the top of the screen when the player enters the scene. This is OPTIONAL. If you don't include this property, we will just not show any text.
        - `objects` LIST: this is a list of the objects in your scene.
    - If it's a `convo`:
        - `img` STRING: this is an image of the object you are interacting with. The object will be in the center of the screen and the background will be the background of where the object was found but blurred out. We will look in your `objects` folder for a file with this name.
        - `text` STRING: this text will display at the top of the dialog menu, above the dialog options.
        - `height` STRING: this is the height of your image. If you do not specify this, the conversation window may not appear in the correct position on the screen.
        - `width` STRING: this is the width of your image. If you do not specify this, the image may not be centered properly.
        - `options` LIST: this is the list of dialog options. Each option is an OBJECT.
- Object objects in scenes look like this:
    - `img`STRING: name of the image for this object. We will look for it in your "objects" folder
    - `x` STRING: distance, in pixels, from the LEFT side of the stage.
    - `y` STRING: distance, in pixels, from the TOP side of the stage (different that on a normal graph, where it's distance from the bottom).
    - `width` STRING: width, in pixels, of the object. If you don't give it a width, but you give it a height, it will set the height and make the width whatever it needs to be to maintain the same aspect ratio. If neither a height or width is specified, the image will not be resized.
    - `height` STRING: see width above, but replace height with width and width with height.
    - `click` STRING: the ID of the scene that will be triggered when this object is clicked on.
    - 'setTrue` STRING OPTIONAL: sets the given variable to true. You shouldn't use this in conjunction with `setFalse`
    - `setFalse` STRING OPTIONAL: sets the given variable to false. You shouldn't use this in conjunction with `setTrue`
    - `if` STRING OPTIONAL: this object will only appear if the variable specified here is true. Should not be used in conjunction with `unless`.
    - `unless` STRING OPTIONAL: this object will not show up if the specified variable is true. Should not be used in conjunction with `if`.

After all is said and done, your `make.json` file should look something like this:
```
{
    "title": "My game!",
    "width": 1920,
    "height": 1080,
    "scenes": [
        {
            "id": "firstScene",
            "type": "location",
            "img": "location.jpg",
            "text": "Welcome to the game?",
            "objects": [
                {
                    "img": "character.jpg",
                    "x": 500,
                    "y": 500,
                    "width": 200,
                    "height": 200,
                    "click": "firstConvo"
                }
            ]
        },
        {
            "id": "firstConvo",
            "type": "convo",
            "img": "character.jpg",
            "height": 200,
            "width": 200,
            "text": "Hello I'm the character",
            "options": [
                "text": "Hello",
                "result": "firstScene"
            ]
        }
    ]
}
```
###Step 4: Generate your game!
When you download this app, it will come with a file called `maker.js` and a folder called `dependencies`. Either copy/move your game folder into the same folder as these files, or move these files so they are in the same folder as your game folder. All that matters is `maker.js`, `yourgame`, and `dependencies` are all in the same folder.

Next, open up a shell. On Windows, you can search in start for `cmd`, open up the run command (windows symbol on your keyboard plus 'r') and run `cmd`, or (in Windows 8+) you can hold the Windows key on your keyboard, hit the 'x' key, then hit the 'c' key. On a Mac, you want to go to spotlight and search for 'terminal' or look for it in your apps folder. Once you have that open, copy the location of your `maker.js` file. For me it was `C:\Users\Dustin\Documents\GitHub\simpleadventure`. Next, you're going to type

```
cd \path\to\your\game
```

and hit 'enter' (but obviously replace the `\path\to\your\game` with whatever you copied. You should be able to right click and hit 'paste' in the shell). Next, type
```
node maker.js nameofyourgame
```
and hit 'enter' (but obviously replace 'name of your game' to whatever your game folder is named). It should say 

```
Game successfully generated as nameofyourgame.html
```
with the 'nameofyourgame.html' being your game's name .html. Now, all you have to do is go into your game folder and open that .html file in a web browser of your choice (but if you want the best results, use Google Chrome version 48 or later, since that's the only browser it has been thoroughly tested in). 

####The app gave me some scary error message
If the app gave you some scary error message instead of saying 'Game successfully generated' then you probably did something wrong. Here are some things to try:
    
 - If you get an error message like

```
module.js:453
    throw err;
    ^

SyntaxError: \path\to\your\game\make.json: Unexpected token
```

  Then your `make.json` file is probably incorrect somewhere. This is usually because you forget a comma or have unbalanced square and curly brackets. A good way to figure out where it went wrong is to use [JSONLint](http://jsonlint.com/). Paste your code in there, hit validate, and fix the errors it tells you about until it says "valid json" in green at the bottom.
    
 - If you get an error message like 

```
    No folder supplied
```
    
  then you did `node maker.js` without putting the name of your game folder.


## I don't know what JSON means
[JSON](http://www.w3schools.com/json/json_syntax.asp) is format for laying out data that's easy for computer programs to understand. Computer programs are really good at understanding things that follow very specific rules, so JSON is a format that has very specific rules. You can learn about the format [here](http://www.w3schools.com/json/json_syntax.asp), or you can read the following rules. Note, however, that the following is not a complete or accurate description of the JSON format. Yet, it is all you need to know for Simple Adventure:
 - There are three "types": `object`, `list`, and `string`
 - A `string` is a collection of `characters`
    - Characters are anything you can make with your keyboard. These are all characters: abc123[{#@&*`~
    - A string in JSON is surrounded by quotes, so `"string"` and consist of any character you can make with your keyboard EXCEPT quotes (`"`) and the backslash (`\`).
        - If you NEED quotes or a backslash in your string, you can use `\"` and `\\`, respectively, and they will be replaced by a double quote symbol and a backslash, respectively.
    - Here are some examples of `strings`:
        - `"abc"`
        - `"1"`
        - `"I am a string"
        - `"I have some !@#$%^ crazy symbols"`
        - `"I have \"quotes\" in me"
 - A `list` is a collection of any number of the other types
    - A `list` can have `objects`, `strings` and even other `lists` inside
    - A `list` in JSON is surrounded by square brackets (`[]`) and the items are separated by commas
    - Here are some examples of `lists`:
        - `[]`
        - `["one item"]`
        - `["two items", "here's the other one"]`
        - `["list with lists", ["I'm a sub list", "I'm something else in the sublist"]]`
- An `object` is like a list, but each item gets a name.
    - The items in an `object` can be any of the other types, just like `lists`, and the item in an `object` can be another `object`
    - An `object` is surrounded by curly brackets (`{}`), and the items go "name" + ":" + "value" and are separated by commas.
    - The "name" MUST be a string and MUST be unique to that object.
        - an object cannot have items with the same name.
    - Here are some examples of `objects`:
        - `{}`
        - `{"name":"value"}`
        - `{"I":"am an object", "I have":"two items"}`
        - `{"colors":["red", "blue", "green"], "fruit":["apple", "orange", "lemon"]}`
- Spaces, tabs, and newlines do NOT matter unless they are inside of strings.
    - The following is legal in JSON
```
{
    "name": "value",
    "otherName": "otherValue",
    "list": [
        "value 1",
        "value 2",
        [
            "sublist",
            {
                "name": "subobject"
            }
        ]
    ]
}
```