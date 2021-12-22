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
