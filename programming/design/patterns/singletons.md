# Overview

Lazy initialized singletons are an easily understood - and thus embraced - pattern.
Unfortunately, they're fucking terrible.
I have yet to meet a singleton I like.

I hate your allocator singleton.

I hate your logger singleton.



# Example

```cpp
#include <cstdio>

#ifndef DEBUG
    const bool Debug = false;
#else
    const bool Debug = true;
#endif

class Logger {
    Logger() {}
public:
    static Logger& GetInstance() {
        static Logger logger;
        return logger;
    }

    void LogLine(const char * message) { puts(message); }
};

class Application {
    Application() {
        if (Debug) {
            Logger::GetInstance().LogLine("Application startup!");
        }
    }

    ~Application() {
        if (Debug) {
            Logger::GetInstance().LogLine("Application shutdown!");
        }
    }
public:
    static Application& GetInstance() {
        static Application app;
        return app;
    }

    void Run() {
        Logger::GetInstance().LogLine("Look ma, bug bait!");
    }
};

int main() {
    Application::GetInstance().Run();
}
```

A classic pattern I've seen in multiple major codebases.  Already, we have problems:
-   `Application` is a global dumping ground of tons of random garbage, causing constant merge conflicts in version control, in my experience.
-   As written, `Logger` is fully constructed first, and thus destroyed last... but only in debug builds.
    In release builds, `Logger` is fully constructed **last**, and thus destroyed **first, before `~Application()` is done using it!**
    This is undefined behavior.
    This incredibly subtle spooky action at a distance tends to result in horrifically brittle behavior that results in build, platform, and configuration specific init and shutdown order bugs.

Another common pattern:
-   The logger singleton relies on the allocator singleton.
-   The allocator singleton relies on the profiler singleton.
-   The profiler singleton relies on the logger singleton.
-   On compilers where static init order is guarded by mutexes, hangs ensue, as the cycle between all 3 singletons means each one wants the others to initialize first.
-   On other compilers, partially initialized objects will be used, resulting in crashes unless the right singleton is used first.

Even more distressing:  I've seen "singletons" that were actually multipletons.
Ever seen an `Allocator::GetInstance()` return something *derived* from `Allocator`, and *other* functions return *non-derived base instances of `Allocator`*?
I have.
Save me from the nightmares!



# Alternative: Ditch the globalish lazy init

```cpp

class Logger {
public:
    Logger() {}
    void LogLine(const char * message) { puts(message); }
};

class Application {
    Logger logger;
    Subsystem1 subsystem1 { logger };
    Subsystem2 subsystem2 { logger, subsystem1 };
    Subsystem3 subsystem3 { logger };
public:
    Application() {
        if (Debug) {
            logger.LogLine("Application startup!");
        }
    }

    ~Application() {
        if (Debug) {
            logger.LogLine("Application shutdown!");
        }
    }

    void Run() {
        logger.LogLine("Look ma, bug bait!");
    }
};

int main() {
    Application app;
    app.Run();
}
```

References to `Logger` can be passed along via dependency injection.
`Application` remains a hot mess, but hey, at least init order is securely defined.
`~Application` won't summon nasal demons.



# Alternative: Free functions

In some cases, it might be reasonable to hide the fact that static data might be in use from the end user.

```cpp
void LogLine(const char * message) { puts(message); }

void Run() {
    LogLine("Look ma, bug bait!");
}

int main() {
    if (Debug) {
        LogLine("Application startup!");
    }

    Run();

    if (Debug) {
        LogLine("Application shutdown!");
    }
}
```

You may still run into issues where e.g. `LogLine` is used before any static data it relies on might be initialized, or after it's torn down... but that can be fixed *inside `LogLine`*, instead of being an issue for the caller to deal with.

```cpp
#include <cstdio>
#include <atomic>

FILE* log_destination = nullptr;

void LogInit() {
    log_destination = fopen("...", "rb");
}

void LogShutdown() {
    fclose(log_destination);
    log_destination = nullptr;
}

void LogLine(const char * message) {
    if (log_destination) {
        fputs(message);
    } else {
        puts(message); // or just ignore logs?
    }
}
```

Now this is merely incredibly thread unsafe during setup, shutdown, and logging.  Progress...?
