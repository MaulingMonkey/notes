# RefCell clone

* [wandbox](https://wandbox.org/permlink/fHd8Tsz1BmBTXlWx)
* [Lifetime Profile Update in Visual Studio 2019 Preview 2](https://devblogs.microsoft.com/cppblog/lifetime-profile-update-in-visual-studio-2019-preview-2/)
* [Warning C26815 The pointer is dangling because it points at a temporary instance that was destroyed. (ES.65)](https://docs.microsoft.com/en-us/cpp/code-quality/c26815?view=msvc-170)
* [F.22: Use T* or owner<T*> to designate a single object](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#f22-use-t-or-ownert-to-designate-a-single-object)

```cpp
// compile with -Werror=dangling

#include <cassert>
#include <vector>
#include <iostream>
#include <type_traits>

namespace gsl {
    template <class T, class = std::enable_if_t<std::is_pointer<T>::value>> using owner = T;
}

template < typename T > class Borrow;
template < typename T > class BorrowMut;

template < typename T > class RefCell {
    RefCell() = delete;
    RefCell(const RefCell &) = delete;

    friend class Borrow<T>;
    friend class BorrowMut<T>;

    T value;
    mutable unsigned borrows = 0u;
public:
    RefCell(T value) noexcept : value(std::move(value)) {}
    RefCell(RefCell&& other) noexcept : value(std::move(other.value)) {}

    Borrow<T> borrow() const noexcept;
    BorrowMut<T> borrow_mut() noexcept;
};

template < typename T > class Borrow {
    friend class RefCell<T>;

    const T & value;
    unsigned & borrows;

    explicit Borrow(const RefCell<T> & refcell) noexcept : value(refcell.value), borrows(refcell.borrows) {
        assert(refcell.borrows != ~0u);
        ++refcell.borrows;
    }

    Borrow& operator=(const Borrow&) = delete;
    Borrow& operator=(Borrow&&) = delete;
    Borrow() = delete;
public:
    Borrow(Borrow&& other) : value(other.value), borrows(other.borrows) { ++borrows; }
    Borrow(const Borrow& other) : value(other.value), borrows(other.borrows) { ++borrows; }
    ~Borrow() noexcept { --borrows; }

    const T& operator*()  const noexcept [[clang::lifetimebound]] { return  value; }
    const T* operator->() const noexcept [[clang::lifetimebound]] { return &value; }
};

template < typename T > class BorrowMut {
    friend class RefCell<T>;

    gsl::owner<T*> const value;
    unsigned* borrows;

    explicit BorrowMut(RefCell<T> & refcell) noexcept : value(&refcell.value), borrows(&refcell.borrows) {
        assert(refcell.borrows == 0u);
        refcell.borrows = ~0u;
    }

    BorrowMut& operator=(const BorrowMut&) = delete;
    BorrowMut& operator=(BorrowMut&&) = delete;
    BorrowMut() = delete;
    BorrowMut(const BorrowMut&) = delete;
public:
    BorrowMut(BorrowMut&& other) : value(other.value), borrows(other.borrows) { other.borrows = nullptr; }
    ~BorrowMut() noexcept { if (borrows) *borrows = 0u; }

    T& operator*()  noexcept [[clang::lifetimebound]] { return *value; }
    T* operator->() noexcept [[clang::lifetimebound]] { return  value; }
};


template < typename T > Borrow   <T> RefCell<T>::borrow    () const noexcept { return Borrow   <T>(*this); }
template < typename T > BorrowMut<T> RefCell<T>::borrow_mut() noexcept       { return BorrowMut<T>(*this); }



int main() {
    RefCell<std::vector<int>> vec(std::vector<int>{ 1, 2, 3 });

    if (false) {
        const auto a = vec.borrow();
        auto b = vec.borrow_mut(); // assertion fail
    }

    if (true) {
        const auto a = vec.borrow();
        const auto b = vec.borrow();
    }

    //auto a = &*vec.borrow(); // error: temporary bound to local reference 'e' will be destroyed at the end of the full-expression [-Werror,-Wdangling]
    //std::cout << a->size() << "\n";

    //int & e = (*vec.borrow_mut())[0]; // error: temporary bound to local reference 'e' will be destroyed at the end of the full-expression [-Werror,-Wdangling]
    //std::cout << e << "\n";
}
```

# Result clone
*   [wandbox](https://wandbox.org/permlink/2YstEhUSXnK2l4aW)
*   Need to dig into std::monostate more
*   Need to see if clang-tidy or --analyze can check for nullable derefs

```cpp
#include <iostream>
#include <variant>
#include <climits>

struct Moved {};
template < typename R > struct Ok  { R value;  Ok(R value): value(std::move(value)) {} };
template < typename E > struct Err { E value; Err(E value): value(std::move(value)) {} };

template < typename R, typename E > class [[nodiscard]] Result {
    std::variant<Moved, Ok<R>, Err<E>> value;

    Result() = default;
public:
    Result(const Result &) = default;
    Result(Result && other) { value.swap(other.value); } // TODO: copy instead if R & E are copyable/POD?
    Result(Ok<R>  value): value(std::move(value)) {}
    Result(Err<E> value): value(std::move(value)) {}

    // TODO: add "_ptr" suffixes, replace unsuffixed versions with std::optional s? those would move/consume - consider adding as_ref() etc.?
    R const * _Nullable ok()  const [[clang::lifetimebound]] { if (auto value = std::get_if<1>(&this->value)) return &value->value; return nullptr; }
    R       * _Nullable ok()        [[clang::lifetimebound]] { if (auto value = std::get_if<1>(&this->value)) return &value->value; return nullptr; }
    E const * _Nullable err() const [[clang::lifetimebound]] { if (auto value = std::get_if<2>(&this->value)) return &value->value; return nullptr; }
    E       * _Nullable err()       [[clang::lifetimebound]] { if (auto value = std::get_if<2>(&this->value)) return &value->value; return nullptr; }
};

template < typename R, typename E > std::ostream& operator<<(std::ostream& os, const Result<R, E> & result) {
    if (auto ok = result.ok()) {
        os << "Result::Ok(" << *ok << ")";
    } else if (auto err = result.err()) {
        os << "Result::Err(" << *err << ")";
    } else {
        os << "Result::Moved";
    }
    return os;
}

Result<int, int> x2(int n) {
    if (n < INT_MIN/2) return Err(n);
    if (n > INT_MAX/2) return Err(n);
    return Ok(2*n);
}

int main(int argc, char **) {
    std::cout << "x2(4)          = " << x2(4) << "\n";
    std::cout << "x2(0x7FFFFFFF) = " << x2(0x7FFFFFFF) << "\n";
    x2(4); // warning: ignoring return value of function declared with 'nodiscard' attribute [-Wunused-result]
    if (argc > 9001) {
        std::cout << "x2(...) = " << *x2(0x7FFFFFFF).ok() << "\n"; // No warning for nullable deref? Boo~
    }
}
```
