## COM Bullshit

Old New Thing:
- [What is COM marshaling and how do I use it?](https://devblogs.microsoft.com/oldnewthing/20151020-00/?p=91321)
- [What are the rules for CoMarshalInterThreadInterfaceInStream and CoGetInterfaceAndReleaseStream?](https://devblogs.microsoft.com/oldnewthing/20151021-00/?p=91311)
- [What are the rules for CoMarshalInterface and CoUnmarshalInterface?](https://devblogs.microsoft.com/oldnewthing/20151022-00/?p=91301)
- [When should I use delayed-marshaling when creating an agile reference?](https://devblogs.microsoft.com/oldnewthing/20191202-00/?p=103171)

cbrumme:
-   [Apartments and Pumping in the CLR](http://web.archive.org/web/20100710155044/http://blogs.msdn.com/b/cbrumme/archive/2004/02/02/66219.aspx)

    > I keep saying that managed blocking will perform “some pumping” when called on an STA thread.  Wouldn’t it be great to know exactly what will get pumped?  Unfortunately, pumping is a black art which is beyond mortal comprehension. 

    > A light-weight way to pump is to call Thread.CurrentThread.Join(0).  This causes the current thread to block until the current thread dies (which isn’t going to happen) or until 0 milliseconds have elapsed – whichever happens first.  I’ll explain later why this also performs some pumping and why this is a controversial aspect of the CLR.

-   https://stackoverflow.com/questions/18487235/wait-for-com-event-to-complete

References:
- [RoGetAgileReference](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-rogetagilereference)
- [CoCreateFreeThreadedMarshaler](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cocreatefreethreadedmarshaler)
- [IGlobalInterfaceTable](https://docs.microsoft.com/en-us/windows/win32/api/objidl/nn-objidl-iglobalinterfacetable) + [Creating the Global Interface Table](https://docs.microsoft.com/en-us/windows/win32/com/creating-the-global-interface-table)
- [IAgileObject](https://docs.microsoft.com/en-us/windows/win32/api/objidlbase/nn-objidlbase-iagileobject)
- [INoMarshal](https://docs.microsoft.com/en-us/windows/win32/api/objidlbase/nn-objidlbase-inomarshal)
- [Marshaling Details](https://docs.microsoft.com/en-us/windows/win32/com/marshaling-details)
- [CoRegisterMessageFilter](https://docs.microsoft.com/en-us/windows/win32/api/objbase/nf-objbase-coregistermessagefilter)
- [CoSetMessageDispatcher](https://docs.microsoft.com/en-us/windows/win32/api/messagedispatcherapi/nf-messagedispatcherapi-cosetmessagedispatcher)([IMessageDispatcher](https://docs.microsoft.com/en-us/windows/win32/api/imessagedispatcher/nn-imessagedispatcher-imessagedispatcher)) "This function is usually called by CoreWindow"
- [combaseapi.h](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/)
- [COWAIT_FLAGS](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/ne-combaseapi-cowait_flags) + CoWaitForMultiple{[Handles](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultiplehandles),[Objects](https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-cowaitformultipleobjects)} - how to dispatch COM?
- [WaitForMultipleObjectsEx](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitformultipleobjectsex)
- [MsgWaitForMultipleObjectsEx](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-msgwaitformultipleobjectsex)
- [Wait functions](https://docs.microsoft.com/en-us/windows/win32/sync/synchronization-functions#wait-functions)
- [QueueUserWorkItem](https://docs.microsoft.com/en-us/windows/win32/api/threadpoollegacyapiset/nf-threadpoollegacyapiset-queueuserworkitem?redirectedfrom=MSDN)
- [Single-Threaded Apartments](https://docs.microsoft.com/en-us/windows/win32/com/single-threaded-apartments)

To Disassemble:
- [CoreDispatcher::ProcessEvents](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.coredispatcher.processevents?view=winrt-19041)

Notes:
- COM Apartment = Thread Group
- [ThreadingModel](https://docs.microsoft.com/en-us/windows/win32/cossdk/threading-model-attribute)

Values:
- `CLSID_SpVoice`           = `{96749377-3391-11d2-9ee300c04f797396}` (`ThreadingModel` = `Both`)
- `ISpObjectTokenCategory`  = `{2d3d3845-39af-4850-bbf940b49780011d)`
- `HKEY_CURRENT_USER\Software\Classes\CLSID`
- `HKEY_CLASSES_ROOT\CLSID`

## [Discord Discussion](https://discord.com/channels/186813135263367169/186813135263367169/743354370296643597)

```
🍊MaulingMonkey🐒Yesterday at 11:24 PM
@💥Washu💥 I have ThreadingModel=Both COM objects I want to poke at from multiple threads... my understanding is that I can theoretically do this from threads belonging to an MTA and the COM object is theoretically supposed to tolerate it?  But:
1. this sounds like a bad thing to rely on, due to the inevitable stupid bugs in COM object implementations? (they are Microsoft COM objects tho)
2. technically I can't access the same objects from STA threads?
3. does COM give me anything to proxy calls from an STA thread, or do I handle that myself?
```

```
💥Washu💥Yesterday at 11:25 PM
MTA should work
STA won't
if you call an MTA com object from STA
I believe it does some magic
But I cannot be certain
```

```
🍊MaulingMonkey🐒Yesterday at 11:26 PM
would that magic kick in just from calling IWhatever methods after creating the IWhatever on an MTA thread?
there's all this documentation about proxies and stubs but I can't for the life of me tell when they kick in :thinking:
```

```
💥Washu💥Yesterday at 11:26 PM
yeah
```

```
🍊MaulingMonkey🐒Yesterday at 11:27 PM
'cause it sounded vaguely like you might get a different IWhatever* that forwards to the real IWhatever*
but... if it's just internal automagic... yay I guess?
```

```
💥Washu💥Yesterday at 11:27 PM
- Every object should live on only one thread (within a single-threaded apartment).
- Initialize the COM library for each thread.
- Marshal all pointers to objects when passing them between apartments.
```

```
🍊MaulingMonkey🐒Yesterday at 11:27 PM
I'm also kinda thinking IWhatever::DoSomething is probably not thread safe, ThreadingModel be damned :thinking:
```

```
💥Washu💥Yesterday at 11:27 PM
for STA
you need to marshal them using
that thing
```

```
🍊MaulingMonkey🐒Yesterday at 11:28 PM
"that thing" :sadparrot:
CoThatThing... >_>
```

```
💥Washu💥Yesterday at 11:29 PM
CoMarshalInterThreadInterfaceInStream or something
```

```
🍊MaulingMonkey🐒Yesterday at 11:29 PM
aha!
```

```
💥Washu💥Yesterday at 11:29 PM
or just CoMarshalInterface
https://docs.microsoft.com/en-us/windows/win32/api/combaseapi/nf-combaseapi-comarshalinterface
CoMarshalInterface function (combaseapi.h) - Win32 apps
Writes into a stream the data required to initialize a proxy object in some client process.

and don't forget CoUnmarshalInterface as well
that's how you marhsal interfaces between STA threads at least
and your STA threads must run a message loop
also see CoGetInterfaceAndReleaseStream
which is easier than the CoUnmarshalInterface stuff
```

```
🍊MaulingMonkey🐒Yesterday at 11:40 PM
excellent... this has also helped me find the relevant oldnewthing blahgs
https://devblogs.microsoft.com/oldnewthing/20151020-00/?p=91321
https://devblogs.microsoft.com/oldnewthing/20151021-00/?p=91311
https://devblogs.microsoft.com/oldnewthing/20151022-00/?p=91301
:book::monkey:
```

# Known COM Values

* `00000000-0000-0000-c000-000000000046` IUnknown
* `00000003-0000-0000-c000-000000000046` IMarshal?
