# Squirrel

I've previously used [Squirrel](http://squirrel-lang.org/) (language) + [Sqrat](https://scrat.sourceforge.net/) (to expose C++ to Squirrel) at [Signal Studios](https://en.wikipedia.org/wiki/Signal_Studios) for gameplay scripting.
It wasn't the worst.



## Pain points

### Mediocre Debugging Experience

We resorted to nonsense such as using Squirrel APIs to dump callstacks via in-process crash handler, which - despite
being fundamentally horrifying - was still an improvement over having no insight into our scripting callstacks on crashes.
This of course did not help with crash dumps, or any issue where the crash handler would crash further or otherwise be skipped.

### Garbage Collection Sandwiches

As is typical with garbage collected gameplay scripting, we had occasional memory issues due to ownership sandwiches.  E.g. if you had:

- a refcounted C++ object owning ...
- a garbage collected Squrrel object owning ...
- a refcounted C++ object owning ...
- a garbage collected Squrrel object owning ...
- a refcounted C++ object

You might need multiple garbage collection cycles for all garbage to actually be collected.
This was particularly gnarly during level transitions, due to the haphazardly interwoven ownership.

### Designer Shenanngians

These can't really be blamed on the language per se, but our designers liked to:
- Give multiple classes in different files the same name, then complain when it didn't work.
- "Inherit" one class, but invoke the constructor of an entirely different class during construction, or skip invoking the base constructor entirely.



## References

- <http://squirrel-lang.org/>
- <https://en.wikipedia.org/wiki/Squirrel_(programming_language)>
- <https://scrat.sourceforge.net/> (sic)
