# C++

**Q:** What is the result of `std::is_signed<decltype(1L * 2U)>()`?<br>
**A:** `sizeof(long) > sizeof(int)`, a compiler and target-arch dependent value.  See [the usual arithmetic conversions](sual_arithmetic_conversions) for details.
<!--
    https://en.cppreference.com/w/c/language/conversion#Usual_arithmetic_conversions
    https://en.cppreference.com/w/c/language/conversion#Integer_promotions
-->

**Q:** How big are member function pointers?<br>
**A:** Up to `4*sizeof(void*)`.  See "Implementations of ..." in [Member Function Pointers and the Fastest Possible C++ Delegates](https://www.codeproject.com/Articles/7150/Member-Function-Pointers-and-the-Fastest-Possible).

**Q:** Null pointers always have a bit pattern of 0, right?<br>
**A:** No.  C++ "Pointers to data member"s are offsets, and 0 is a valid offset.  `nullptr`/`(int C::*)0` may have a bit pattern of `-1`

# Array Co/contra variance type hole (C#, Java)

```java
// Java
class Base {}
class Derived1 extends Base {}
class Derived2 extends Base {}
Base[] array = new Derived1[1]; // Legal implicit cast
array[0] = new Derived2(); // ArrayStoreException 
```

```csharp
// C#
class Base {}
class Derived1 : Base {}
class Derived2 : Base {}
Base[] array = new Derived1[1]; // Legal implicit cast
array[0] = new Derived2(); // ArrayTypeMismatchException
```

Leads to some fun "optimizations" in C#, as shared by [Washu](https://discord.com/channels/186813135263367169/186813135263367169/912149381069824000):
```csharp
struct Derived1Holder { public Derived1 Instance; }

static void OptimizedAssigner(Derived1Holder[] array) {
    array[0].Instance = new Derived1(); // under the hood bounds check, but no under the hood type-check
}

static void SlowerAssigner(Derived1[] array) {
    array[0] = new Derived1(); // under the hood bounds check *and* type check
}
```

# ARM

* [ARM PC register points two instructions ahead](https://stackoverflow.com/questions/24091566/why-does-the-arm-pc-register-point-to-the-instruction-after-the-next-one-to-be-e/24092329#24092329)
