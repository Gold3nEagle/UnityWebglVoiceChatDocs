# VoiceChat Class

## constructor
instantiate the class

```javascript
var voiceChat = new VoiceChat();
```

## Events
Nan

## Attributes

### voiceChat.io
contains the socket io connection of the current client. learn more [here](https://socket.io/docs/v4/client-api/).

### voiceChat.hark
contains an instance of hark (used for speech detection). learn more [here](https://github.com/latentflip/hark).

### voiceChat.isMute
the mute status of the current client.

### voiceChat.rooms
contains a list of the current room(s) the client is connected to.

### voiceChat.muteList
contains a list of the other clients the current client has muted locally.

### voiceChat.timeInterval
the interval time used between recordings to be sent through the network.

### voiceChat.tmpClientsInRoom
this is a temporary variable for storing the clients id of a requested specfic room and is only relevant when the function is called.

### voiceChat.UnityInstance
this will be set by exposing the unity instance after it's creation in the window variable to enable access in this class.

it is used to allow communtication from the browser to Unity game build.

### voiceChat.MicrohphoneManager
this is the microphone manager component / class that will take care of managing the microphone stream and change of the microphone

### voiceChat.ChatManager
this is the Chat manager component / class that will take care of sending / receiving messages for the room/s the player is in.


***


## Methods

### Internal Functions (private)
_the below functions are internally used within the class itself and does not need to be called._

#### voiceChat.Chat()
starts the voice stream and sends it to the server (only needs to run once, automatically runs with the constructor).

#### voiceChat.EmitVoice(ArrayBuffer)
will take an array buffer (base64 encoded audio) and emit it to the socket server, the socket server has a list of the rooms the player is in.

#### voiceChat.RecordAudio(MediaStream)
this function takes a mediaStream source starts the recording loop and returns an ArrayBuffer.

#### voiceChat.InitHark(MediaStream)
this function takes a MediaStream and initializes hark with it alongside registering speech detection events.

***

### External Functions (public)
_the below functions are meant to be used externally by other scripts._

#### voiceChat.IsSpeaking()
returns a boolean of whether the current client is speaking or not.

#### voiceChat.GetMySocketID()
returns a string of the current client socket id

#### voiceChat.SetMute(boolean)
takes a boolean to mute/unmute the current client from others

#### voiceChat.IsOtherClientMuted(string clientID)
takes a string of another client ID and checks it they are muted by the local client (local mute)

#### voiceChat.MuteClient(string clientID)
takes a string of another client ID and mutes them locally for the current client

#### voiceChat.UnmuteClient(string clientID)
takes a string of another client ID and unmutes them locally for the current client

#### voiceChat.UnmuteAllClients()
unmutes them locally for the current client

#### voiceChat.GetRooms()
returns a list of all the rooms the current user is currently at.

#### voiceChat.GetClientsInRoom(string roomID)
returns a list of all clients in the provided room.

#### voiceChat.JoinRoom(string roomID)
takes a room id and create/join the current user to the room id provided.

#### voiceChat.LeaveRoom(string roomID)
takes a room id and disconnects the current user from the room id provided if they are in that room.

#### voiceChat.LeaveAllRooms()
disconnects the current user from all the rooms they have joined.

***

# Microphone Class

## Constructor
```javascript
this.MicrohphoneManager = new Microhphone(voiceChatInstance);
```
the constructor takes an instance of the voice chat class 

## Events

### streamAvailable
this event will be triggerd whenever a stream is ready / changed and will send the stream as a parameter

## Attributes

### Microhphone.voiceChat
a refrence of the voicechat instance

### Microhphone.localStream
a refrence of the current localStream (microphone stream)

### Microhphone.microhphonesList
stores a list of the available devices of type "audioinput" so that the user/player can use it to switch / pick one to be used/changed with.

### Microhphone.isPermited
stores a refrence of whethere the player/user have access to the microphone


***


## Methods

### CanAccessMicrophone()
this function returns a promise with a boolean of whethere is the user is permmited to access the microphone.

### GetAvailableMicrophones()
returns a promise with a list of all the available audioinput devices
```javascript 
Promise<MediaDeviceInfo[]> 
```

### HandleMicrophoneChange(stream, microphoneId)
changes the currently used stream. takes the current stream if available and the new audioinput device id as params and closes the current stream tracks than replace the current stream with the new one by the provided device id.

### GetLocalStream()
fetches the current stream from the user using the default audioinput device set by the OS.


***

# ChatManager Class

## Constructor
```javascript
this.ChatManager = new ChatManager(voiceChatInstance);
```
the constructor takes an instance of the voice chat class 

## Events
Nan

## Attributes
Nan

## Methods

### SendMessage(message)
this funcition takes a string as a parameter for the message and emits it to the room/s the player is in.

### OnUpdateChatHandler(message, clientID)
this fucntion takes two parameters, the message and the client id whom sent the message then sends it back to the unity instance. the function will work whenever a new message recived from the another client.
