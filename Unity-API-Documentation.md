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

## Events

**\*\*Important Note\*\***

the reason why the class contains events is that some functionality requires asynchronous processes to run in order to fetch some data over the network and since unity doesn't support asynchronous functions or to say more accurately. It can't wait for an async function from JavaScript to return a value, for there is no direct way for the game (C#) to know that such a process is running in a different language, and thus can't wait for an undefined amount of time between two languages.

a solution for this would be to allow the browser to send messages to unity which is [possible](https://docs.unity3d.com/Manual/webgl-interactingwithbrowserscripting.html#:~:text=src/library*.-,Code%20visibility,-The%20recommended%20approach), but in order to achieve that, a modification on the game JavaScript code needs to be done every time a build is done since unity 2019 and newer which adds more steps to the setup alongside exposing all of unity current GameObjects to be called from the browser with their attached scripts functions (Security Concern).

and hence the current events.

***

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

#### UpdateClientsInRoom(string roomID)

```C#
public void UpdateClientsInRoom(string roomID)
```
the function runs a process where it fetches the clients in a specified room and then triggers an event (onGetClientsInRoom) after a specified time.

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

### Private Methods

***

#### GetClientsInRoom()

```C#
private List<string> GetClientsInRoom()
```
returns a list of the clients in a room that was prefetched from "UpdateClientsInRoom" function.

**Returns**
```C#
List<string>
```

***

#### FetchClientsAfterSomeTime()

```C#
private IEnumerator FetchClientsAfterSomeTime()
```
this coroutine starts inside "UpdateClientsInRoom" to allow unity to wait for a portion of time before trying to get the clients of a room, after that time. it will run "GetClientsInRoom" function receiving the list and invoking the event "onGetClientsInRoom" that will notify subscribers with the data

***

#### IsSpeakingChecker()

```C#
private IEnumerator IsSpeakingChecker()
```
this coroutine starts at the beginning of the game (SingletonStarted) function. It periodically updates the status of the is speaking using the trigger below.

```C#
onIsSpeaking?.Invoke(IsSpeaking());
```
any game object can subscribe to it to see if the player/client is currently speaking or not.
