# C++

**Q:** What is the result of `std::is_signed<decltype(1L * 2U)>()`?<br>
**A:** `sizeof(long) > sizeof(int)`, a compiler and target-arch dependent value.
<!--
    https://en.cppreference.com/w/c/language/conversion#Usual_arithmetic_conversions
    https://en.cppreference.com/w/c/language/conversion#Integer_promotions
-->
