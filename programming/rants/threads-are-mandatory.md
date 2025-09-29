# Threads are Mandatory
<!-- original: https://news.ycombinator.com/item?id=23175742 -->

> You SHOULD NOT use threads.

You have no choice.

Graphics drivers fire up threads. Audio libraries fire up threads. Xinput fires up threads. Your library will be used in multithreaded programs. "I have an executable!" you might object - but this will be rebuilt as a library to accomplish involuntary multithreading if need be, or just loaded like a DLL as-is. Anti-viruses will inject threads. Corporate monitoring tools will inject threads. Debuggers will inject threads. You'll need to be able to initialize COM cleanly without worrying what the impact on other COM APIs the current thread could be using might be. You'll need to turn a blocking API into an event loop or other asyncronous style. Android fires up separate UI and Rendering threads even if you don't want them, and IIRC prohbiits some IO on the rendering thread. WinRT's main UI thread isn't the "main" thread that ran the entry point.

You can avoid firing up a single thread and *still* end up debugging multithreading bugs. Given that you're going to have to go through the effort of making decent chunks of your code multithreading safe anyways, you might as well use some of the easier tools for the embarassingly parallel parts of your program for some easy performance wins.
