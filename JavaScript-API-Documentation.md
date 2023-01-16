# VoiceChat

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

### voiceChat.audioStream
contains the MediaStream object fetched from the mic.

### voiceChat.rooms
contains a list of the current room(s) the client is connected to.

### voiceChat.muteList
contains a list of the other clients the current client has muted locally.

### voiceChat.timeInterval
the interval time used between recordings to be sent through the network.


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
