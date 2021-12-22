# RefCell clone

* [wandbox](https://wandbox.org/permlink/fHd8Tsz1BmBTXlWx)

```cpp
// compile with -Werror=dangling

#include <cassert>
#include <vector>
#include <iostream>

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
    RefCell(T value): value(std::move(value)) {}
    RefCell(RefCell&& other): value(std::move(other.value)) {}

    Borrow<T> borrow() const;
    BorrowMut<T> borrow_mut();
};

template < typename T > class Borrow {
    const RefCell<T> & refcell;
    friend class RefCell<T>;
    Borrow(const RefCell<T> & refcell): refcell(refcell) {
        assert(refcell.borrows != ~0u);
        ++refcell.borrows;
    }
public:
    ~Borrow() { --refcell.borrows; }

    const T& operator*()  const [[clang::lifetimebound]] { return refcell.value; }
    const T* operator->() const [[clang::lifetimebound]] { return &refcell.value; }
};

template < typename T > class BorrowMut {
    RefCell<T> & refcell;
    friend class RefCell<T>;
    BorrowMut(RefCell<T> & refcell): refcell(refcell) {
        assert(refcell.borrows == 0u);
        refcell.borrows = ~0u;
    }
public:
    ~BorrowMut() { refcell.borrows = 0u; }

    T& operator*()  [[clang::lifetimebound]] { return refcell.value; }
    T* operator->() [[clang::lifetimebound]] { return &refcell.value; }
};


template < typename T > Borrow   <T> RefCell<T>::borrow    () const { return Borrow   <T>(*this); }
template < typename T > BorrowMut<T> RefCell<T>::borrow_mut()       { return BorrowMut<T>(*this); }



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
