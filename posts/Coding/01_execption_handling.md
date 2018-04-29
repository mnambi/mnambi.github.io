Exception Handling
===
https://isocpp.org/wiki/faq/exceptions

- Throwing an execption is one of the basis of RAII (Resource Acquisition Is Initialization). 
- You don't have to think about error codes which can be tricky. 
- Exceptions are relatively costly. Should not be used in hard-real time and safety-critical applications. 
  “But in JSF++ Stroustrup himself bans exceptions outright!”
- Exceptions are used to signal errors that cannot be handled locally such as acquiring a resource. 
```
class VectorInSpecialMemory {
    int sz;
    int* elem;
public:
    VectorInSpecialMemory(int s) 
        : sz(s) 
        , elem(AllocateInSpecialMemory(s))
    { 
        if (elem == nullptr)
            throw std::bad_alloc();
    }
    ...
};
```

- Do not use throw to indicate a coding error in usage of a function. Use assert or other mechanism to either send the process into a debugger or to crash the process and collect the crash dump for the developer to debug.
- Do not use throw if you discover unexpected violation of an invariant of your component, use assert or other mechanism to terminate the program. Throwing an exception will not cure memory corruption and may lead to further corruption of important user data.
- Exceptions are great for error propagation. 

