# Installation

On this page, there are two parts that are essential for the voice chat application to work properly. the first is hosting the voice chat server. the second is adding the files in your unity game and setting up some variables.



## Hosting the server

To begin with, clone the repo using

``
git clone https://github.com/Gold3nEagle/UnityWebGLVoiceChatServer-Socket.io.git
``

pre-requisites: -

- npm
- node
- nodemon (optional)
- server running on HTTPS (required to get access to the mic)

to run the server use

``
npm run dev
``

## Importing unity files

### giving access to the unity instance to the browser (optional)

_**this is optional**_, for, at the moment, the browser does not need to send messages to the unity game instance

also, make sure that you store your "unityInstance" variable on the "window" variable
like this

note: this section of the code is found with the build of the WebGL game
```javascript
  }).then((unityInstance) => {
          window.unityInstance = unityInstance;
```


### adding the .jslib file and using the VoiceChatHandler.cs Class

The two files in the UnityFiles Folder are meant to be used in your unity game.

Those two files will allow unity to communicate with the browser where the .jslib acts as the bridge and the WebVoiceChatHandler act as the interface to use the features.

The VoiceChatHandler class only requires the user to provide two links, the first link will be used to connect to the server. The other link will be used to fetch the client.js file from the server which contains the browser logic to start the voice chat.

for more info check the wiki of the repo about the files.

## How The voice chat starts

After adding the unity files to your game and providing the required URLs, you will have access to all the functionality of the voice chat library. However, none of them will be functional in the editor, for the functionality itself is executed on the browser with JavaScript.

the way it works is that when the game loads, the VCH (VoiceChatHandler) will instantiate itself and stay persistent throughout the game. On Start, the class will use the provided URLs from the class (in the editor) to do two things.

the first URL will be used as the domain to connect to the voice chat server, and the other URL will be the location of the voice chat script which will be used to append the script to the current HTML page by unity.

when the file loads, the class will be ready to be used.