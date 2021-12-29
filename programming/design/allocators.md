# Considerations

*   Call site debug info
*   IAllocator chaining
*   Interface flattening
*   Allocation prefixing
*   Debug/stats
*   Rc cycle tracking
*   ...

# Minimal Interface

*   Most APIs can get away with a malloc/free pair.
*   Middleware might make malloc/free a callback to allow programs to override library allocs.

# Maximal Interface

```cpp
// allocators.h
struct IAllocator {
    virtual ~IAllocator() {}
    
    virtual void* alloc      (size_t align, size_t size) const = 0;
    virtual void* alloc_debug(size_t align, size_t size, const CallSiteInfo &) const { return alloc(align, size); }
    virtual void  free       (void*) const = 0;
    virtual void  free_debug (void* ptr, const CallSiteInfo &) const { return free(ptr); }
    // ...
};

struct BasicNoLoggingAllocator : IAllocator { ... };
struct MyFancyDiagnosticAllocator : IAllocator { ... };
```

```cpp
// init.cpp
BasicNoLoggingAllocator alloc_basic;

// You might give your low level logging - used for asserts/bugs/stacktraces - a dedicated allocator.
// This prevents log => alloc => log => alloc loops.
// Having dedicated, possibly pre-allocated memory for logging will also reduce the chance of it failing when self-reporting heap corruption or other issues.
BasicNoLoggingAllocator alloc_logs;
LowLevelLoggingSystem lllog(alloc_logs);

// Could give allocators per-system
NormalAllocator alloc_normal(alloc_basic, lllog); // might only log/assert in exceptional heap corruption cases
HeavyDebugAllocator alloc_debug(alloc_normal, lllog); // might log every single allocation
// make DEBUG_*_ALLOCS constexpr in release builds for optimization? be explicit about types for release builds?
const IAllocator & system1 = DEBUG_SYSTEM1_ALLOCS ? alloc_debug : alloc_normal;
const IAllocator & system2 = DEBUG_SYSTEM2_ALLOCS ? alloc_debug : alloc_normal;
const IAllocator & system3 = DEBUG_SYSTEM3_ALLOCS ? alloc_debug : alloc_normal;
const IAllocator & system4 = DEBUG_SYSTEM3_ALLOCS ? ExtraDebugAllocator("system4", alloc_debug) : alloc_normal;
```

```cpp
// Support junk

#include <iostream>
#include <string_view>

template < typename T > constexpr std::string_view type_name() {
    const std::string_view f = __PRETTY_FUNCTION__;
    const size_t begin = f.find("[T = "); // Find e.g. `std::string_view type_name() [T = int]
    return (begin != std::string_view::npos) ? f.substr(begin+5, f.size()-begin-6) : f;
}

#define CALL_SITE_INFO() CallSiteInfo { __FILE__, __LINE__, __PRETTY_FUNCTION__, type_name<int>() }
struct CallSiteInfo {
    const std::string_view file; // = __FILE__
    const size_t           line; // = __LINE__
    const std::string_view func; // = __PRETTY_FUNCTION__ / __FUNCTION__
    const std::string_view type; // = type_name<T>();
};
```
