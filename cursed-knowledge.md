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
