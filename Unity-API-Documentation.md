# VoiceChatHandler

this class is a singleton that can be used anywhere in a unity game project which handles communication with the voice chat instance of JavaScript in the browser, this is how the Singleton was achieved in unity with mono behavior functionality, [here](https://gist.github.com/rickyah/271e3aa31ff8079365bc).

## Attributes

### socketServer
```C#
public string socketServer
```
This is going to be the variable that will hold the socket server URL which is going to be used in the build to connect the socket server

***

### clientJSFileURL
```C#
public string clientJSFileURL
```
the URL link to the client js file that contains the voice chat logic for the browser. This will be used to append the code from the URL to the current page the game is hosted on.

***
### clientsInCurrentRoom
```C#
private List<string> clientsInCurrentRoom
```
holds the list of of clients in the current joined room. (will be updated automatically from the browser).

***
### availableMicrophones
```C#
private List<Microphone> availableMicrophones
```
stores a list of the availabe microphones

***
### isSpeaking
```C#
private bool isSpeaking
```
an indicator of whether the client is speaking or not. (will be updated automatically from the browser).


***

## Events

### OnClientsInRoom
```C#
public UnityEvent<List<string>> OnClientsInRoom
```
this event will be triggered when the browser sends back the list of clients in the room

**Returns**
```C#
List<string>
```

***

### onIsSpeaking
```C#
public UnityEvent<bool> onIsSpeaking
```
when invoked, returns a string of a JSON with the status of whether is the current client is talking or not.

tip: this value can be used with other networking framework solutions(mirror, fishnet, smartfox, etc...) to send the player status to other in-game players.

**Returns**
```C#
bool
```

***

### OnAvailableMicrophones
```C#
public UnityEvent<List<Microphone>> OnAvailableMicrophones
```
this event is triggered when the browser sends back a list of available microphones (audioinput)

**Returns**
```C#
List<Microphone>
```

***

### OnCanAccessMicrophone
```C#
public UnityEvent<bool> OnCanAccessMicrophone
```
this event is triggered when the browser sends back a response for whethere if you the player/user have provided access to the voice chat script.

**Returns**
```C#
bool
```

***

### OnMessageReceived
```C#
public UnityEvent<string,string> OnMessageReceived
```
this event is triggered when the browser sends back a response with a message received. th first parameter represents the clientID who sent the message and the second parameter represents the actual message itself.

**Returns**
```C#
<string, string>
```

***

## Methods

### Public Methods

#### GetMySocketID()

```C#
public string GetMySocketID()
```
this function returns a string of the current client socket-io id which can be used directly as a string

e.g: -

`
"w2YTHXzcN2fJCuROAAAB"
`

**Returns**
```C#
string
```

***

#### IsVoiceChatReady()

```C#
public string IsVoiceChatReady()
```
this function returns a string of whether the voice chat browser app is ready to be used or not

e.g: -

`
"yes"
or
"no"
`

**Returns**
```C#
string
```

***

#### MuteMic()

```C#
public void MuteMic()
```
this function mutes your mic.

***

#### UnMuteMic()

```C#
public void UnMuteMic()
```
this function unmutes your mic.

***

#### IsMute()

```C#
public bool IsMute()
```
this function returns a bool indicating whether if the mic is mute or not

**Returns**
```C#
bool
```

***

#### SetIsSpeaking(string isSpeaking)

```C#
public void SetIsSpeaking(string isSpeaking)
```

sets the is speaking paramater and invokes the onIsSpeaking event. (the browser will update the list using the function)

***
#### IsSpeaking()

```C#
public bool IsSpeaking()
```
this function returns bool of whether the current user is speaking or not regardless of whether the mic is mute or not

**Returns**
```C#
bool
```

***
#### GetAvailableMicrophones()
```C#
public void GetAvailableMicrophones()
```
sends a request to the browser the fetch all the available microphones.
the resoponse is sent back to "SetAvailableMicrophones()" function.

***
#### SetAvailableMicrophones()
```C#
public void SetAvailableMicrophones(string microphonesJson)
```
this funcition is called by the browser which sends back the list of microphones fetched.

the function itself populates "availableMicrophones" variable and triggers the "OnAvailableMicrophones" event, sending the list to whatever function is listening to it.

***
#### SetCurrentMicrophone()
```C#
public void SetCurrentMicrophone(Microphone microphone)
```
this function sends a request to the browser to change the currently used microphone from the browser. it takes a "VoiceChatSolution.Microphone" struct as a parameter.

***
#### CanAccessMicrophone()
```C#
public void CanAccessMicrophone()
```
sends a request to the browser about whether the browser has access to the microphone or not. the resoponse is set back in "CanAccessMicrophoneResponse" function.

***
#### CanAccessMicrophoneResponse()
```C#
public void CanAccessMicrophoneResponse(string canAccess)
```
this function is called by the browser where it is provided with an object that holds a boolean of whether the browser has access or not.

this function also triggers "OnCanAccessMicrophone" event sending the result back to whatever function is listening to it.

***

#### GetRooms()

```C#
public List<string> GetRooms()
```
this function returns a list of strings of the rooms the player is currently joined in.

furthermore, it refreshes the list of rooms. the new list of rooms is then set by browser through "SetRooms" function.

**Returns**
```C#
List<string>
```

***

#### SetRooms(string roomsJson)

```C#
public void SetRooms(string roomsJson)
```
sets the list of the rooms the client is in.

***

#### UpdateClientsInRoom(string roomID)

```C#
public void UpdateClientsInRoom(string roomID)
```
the function runs a process where it fetches the clients in a specified room, than the browser will update the list using "SetClientsInRoom" function

***

#### SetClientsInRoom(string clients)

```C#
public void SetClientsInRoom(string clients)
```

sets the list of clients in the current room the user/player is in. (the browser will excute this function)

the event "OnClientsInRoom" is triggered with list of the clients.


***

#### JoinRoom(string roomID)

```C#
public void JoinRoom(string roomID)
```
the function joins/creates and joins the player in the voice chat room

***

#### JoinOneRoom(string roomID)

```C#
public void JoinOneRoom(string roomID)
```
same as the one above but it makes sure that you leave rooms before joining the given one

***

#### LeaveRoom(string roomID)

```C#
public void LeaveRoom(string roomID)
```
disconnects the client from the room id given

***

#### LeaveAllRooms(string roomID)

```C#
public void LeaveAllRooms()
```
disconnects the client from all the rooms they are connected to.

***

#### MuteOtherClient(string clientID)

```C#
public void MuteOtherClient(string clientID)
```
locally mutes another client for the current client.

***

#### UnMuteOtherClient(string clientID)

```C#
public void UnMuteOtherClient(string clientID)
```
locally unmutes another client for the current client.

***

#### UnMuteAllClients(string clientID)

```C#
public void UnMuteAllClients()
```
locally unmutes all muted clients for the current client.

***

#### IsOtherClientMuted(string clientID)

```C#
public bool IsOtherClientMuted(string clientID)
```

this function returns a bool indicating whether if the provided client is muted locally or not by the current user.

**Returns**
```C#
bool
```

***

#### GetMuteList()

```C#
public List<string> GetMuteList()
```

returns a list of strings of all the locally muted other clients

**Returns**
```C#
List<string>
```

***
#### SendChatMessage(string message)
```C#
public void SendChatMessage(string message)
```
takes a string as the message and sends it to the browser to be sent to other clients of the same room.

***
#### ReceiveChatMessage(string jsonMessage)
```C#
public void ReceiveChatMessage(string jsonMessage)
```

this function is it triggerd by the browser which sends any message that is intended for the current client / player.

this function also triggers the "OnMessageReceived" event sending the data to whatever function is listening to it.

***
***

## Structures

### Microphone
```C#
public struct Microphone
{
    public string deviceId;
    public string kind;
    public string label;
    public string groupId;
}
```

this structure is used to allocate the microphone meta data within the VoiceChatHandler Script.