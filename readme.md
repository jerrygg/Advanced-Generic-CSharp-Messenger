# Advanced Generic C# Messenger
Advanced Generic C# Messenger is a messaging system originally developed for Unity components to communicate with each other in a loosely coupled way.
## Features:
* Create an event manager for any Enum based on the `System.Int32 (int)` type
* Option to log all messages
* Use of generics provides error detection and safeguards
* Create global event managers that can be accessed by any script (by using the repository)

## Usage:

Copy the `AdvancedGenericCSharpMessenger` folder into your Unity project assets folder to include the scripts.

1. To create a new event (this assumes you have an enum named GlobalEvent):
 	```csharp
    EventManager<GlobalEvent> globalEventManager = new EventManager<GlobalEvent>();
    ```
 		
 2. To add a listener to an eventManager:
    ```csharp
    //adds a listener with no parameters passed to registered callback.
    globalEventManager.AddListener(GlobalEvent.DoSomething, FunctionToHandleDoSomethingEvent);
    //adds a listener that expects one parameter passed to registered callback.
    globalEventManager.AddListener<float>(GlobalEvent.DoSomething, FunctionToHandleDoSomethingEventWithFloatParamater);
    ```
 	
 3. To broadcast an event
    ```csharp
    //Broadcasts a message with no parameters
 	globalEventManager.Broadcast(GlobalEvent.DoSomething);
 	//Broadcasts a message with one parameter passed to registered callbacks.
 	globalEventManager.Broadcast<float>(GlobalEvent.DoSomething, 4f);
 	```
 
 4. Accessing a single instance event via the `EventManagerRepository`. Single instance events are useful if you need to be able to access the same event manager from many different scripts.
    ```csharp
    A) EventManagerRepository.Instance.GetSingleInstanceEventManager<GlobalEvent>().Broadcast<float>(GlobalEvent.DoSomething, 4f);
    ```
    If you are accessing an event manager in the repository frequently, store a reference in the script where you need it instead of using the `GetSingleInstanceEventManager` method.

### EventManager Persistence
 
The repository cleans up all event managers automatically when a new level is loaded. By default, all events are cleared from each event manager and the managers are removed from the repository.

If you want an event to survive a level load, mark it as persistent via the MarkEventAsPersistent method, like so:
```csharp
globalEventManager.MarkEventAsPersistent(GlobalEvent.DoSomething);
```
If later you want the event to not survive a level load, you can use the `MarkEventAsInterim` method in the same manner.

Alternatively, you can set the SaveAllEvents value of an event manager to true if you want all events to survive a level load, like so:
```csharp
globalEventManager.SaveAllEvents = true;
```
Please note, this does not mark each event as persistent, instead it simply tells the event manager to skip the cleanup phase when a level is loaded.
So later, if you set `SaveAllEvents` to false, and there are no persistent messages, the event manageer will be removed
from the repository when a new level is loaded. 

Regardless of `SaveAllEvents` value, you can change an event to/from persistent/interim at any time (but note an events persistent/interim status only matters when `SaveAllEvents` is false).

One final caveat. The repository removes event managers automatically upon level load (as long as the manager has no persistent events and its `SaveAllEvents` value is set to false), or when `RemoveEventManager` is called, but this does not clear the event manager from memory. 

If you have a script which references the eventManager, it will remain in memory. Always make sure to get a fresh single instance event manager from the repository if you expect a new version, and null out any references to event managers when you no longer are using them. The cleanup process is designed to try and limit errors between different levels, but it is not completely error proof. Some knowledge of delegates/c# memory management is required to avoid exceptions.

#### Legal

* Original by Rob Hyde [CSharpMessenger](http://wiki.unity3d.com/index.php/CSharpMessenger) 
* Modified by Magnus Wolffelt [CSharpMessenger Extended](http://wiki.unity3d.com/index.php/CSharpMessenger_Extended) 
* Modified again by Ilya Suzdalnitski [Advanced CSharp Messenger](http://wiki.unity3d.com/index.php/Advanced_CSharp_Messenger)
* Modified and packaged by user gilley033 [Advanced Generic C# messenger ](http://forum.unity3d.com/threads/advanced-c-messenger.112449/) 
* Readme created, file directories simplified, and redistributed by Michael Nam under [creative commons share alike.](https://creativecommons.org/licenses/by-sa/3.0/)

###### Disclaimer:
###### *UNLESS OTHERWISE MUTUALLY AGREED TO BY THE PARTIES IN WRITING, LICENSOR OFFERS THE WORK AS-IS AND MAKES NO REPRESENTATIONS OR WARRANTIES OF ANY KIND CONCERNING THE WORK, EXPRESS, IMPLIED, STATUTORY OR OTHERWISE, INCLUDING, WITHOUT LIMITATION, WARRANTIES OF TITLE, MERCHANTIBILITY, FITNESS FOR A PARTICULAR PURPOSE, NONINFRINGEMENT, OR THE ABSENCE OF LATENT OR OTHER DEFECTS, ACCURACY, OR THE PRESENCE OF ABSENCE OF ERRORS, WHETHER OR NOT DISCOVERABLE. SOME JURISDICTIONS DO NOT ALLOW THE EXCLUSION OF IMPLIED WARRANTIES, SO SUCH EXCLUSION MAY NOT APPLY TO YOU.*
###### *EXCEPT TO THE EXTENT REQUIRED BY APPLICABLE LAW, IN NO EVENT WILL LICENSOR BE LIABLE TO YOU ON ANY LEGAL THEORY FOR ANY SPECIAL, INCIDENTAL, CONSEQUENTIAL, PUNITIVE OR EXEMPLARY DAMAGES ARISING OUT OF THIS LICENSE OR THE USE OF THE WORK, EVEN IF LICENSOR HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.*

