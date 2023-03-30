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

### clientsInCurrentRoom
```C#
private List<string> clientsInCurrentRoom;
```
holds the list of of clients in the current joined room. (will be updated automatically from the browser).

***
### isSpeaking
```C#
private bool isSpeaking;
```
an indicator of whether the client is speaking or not. (will be updated automatically from the browser).


***

## Events

### onGetClientsInRoom
```C#
public UnityEvent<List<string>> onGetClientsInRoom
```
this event will be triggered after some time from calling "UpdateClientsInRoom(string roomID)" function passing a JSON object with a list of the clients in a specified room as a string.

example of returned string: -

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

#### GetRooms()

```C#
public List<string> GetRooms()
```
this function returns a list of strings of the rooms the player is currently joined in

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

sets the list of clients in the current room the client is in. (the browser will excute this function)


***

#### JoinRoom(string roomID)

```C#
public void JoinRoom(string roomID)
```
the function joins/creates and joins the player in the voice chat room

***

#### JoinOneRoom(string roomID)

```C#
public void JoinRoom(string roomID)
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

