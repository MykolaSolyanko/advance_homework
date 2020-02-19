## Реализовать класс умный указатель, который уникально владеет ресурсом
Необходимо шаблонную реализацию класса `Unique_ptr`. Этот класс может только уникально владеть ресурсом, т.е. запрещено копировать объект этого класса, можно только перемещать.
Этот класс также должен принимать как параметр шаблона функцию deleter для ресурса. Например, если у нас наш объект класса `Unique_ptr` владеет указателем на файл `FILE*`, то ему нужно передать как аргумент функцию `fclose`. Если у нас объект класса владеет ресурсом который выделен был через оператор `new`, то по дефолту он в деструкторе должен вызывать `delete`. Если у нас объект класса владеет ресурсом который выделен был через оператор `new[]`, то по дефолту он в деструкторе должен вызывать `delete[]`. Т.е. класс `Unique_ptr` по умолчанию использует как deleter ресурса `delete` или `delete[]`, а иначе если ему передали явно пользовательский deleter то необходимо вызывать его.
 
```
Unique_ptr<int> ptr1{new int{}}; // use ~Unique_ptr() {delete ptr;}
Unique_ptr<int[]> ptr2{new int[200]{}}; // use ~Unique_ptr() {delete[] ptr;}
 
....
auto file = fopen(file_name);
Unique_ptr<File> ptr{file, fclose}; // use ~Unique_ptr() {fclose(ptr);}
 
...
 
Unique_ptr<int> ptr_{new int{}};
 
Unique_ptr<int> ptr_copy{ptr}; // compiler error
Unique_ptr<int> ptr_copy{std::move(ptr)}; // compiler OK
```
 
 
## Реализовать класс умный указатель, который колективно(shared) владеет ресурсом
Необходимо шаблонную реализацию класса `Shared_ptr`. Этот класс может коллективно(раздельно) владеть ресурсом, т.е. объект этого класса можно как копировать, так и перемещать. Идея разделяемого умного указателя базируется на таком понятии как счетчик ссылок, когда этот счетчик будет равен 0, то ресурс необходимо освободить используя для этого или пользовательский deleter, или установленный по умолчанию.
Также как и для класса `Unique_ptr` необходимо реализовать возможность передачи пользовательского deleter. Поведение такое как и при классе `Unique_ptr`.
 
```
Shared_ptr<int> ptr1{new int{}}; // use ~Shared_ptr() {delete ptr;}
Shared_ptr<int[]> ptr2{new int[200]{}}; // use ~Shared_ptr() {delete[] ptr;}
 
....
auto file = fopen(file_name);
Shared_ptr<File> ptr{file, fclose}; // use ~Shared_ptr() {fclose(ptr);}
 
...
 
Shared_ptr<int> ptr_{new int{}};
 
std::cout << ptr.count() << std::endl; // print 1
 
Shared_ptr<int> ptr_copy{ptr};
 
std::cout << ptr_copy.count() << std::endl; // print 2
Unique_ptr<int> ptr_move{std::move(ptr)};
 
std::cout << ptr_copy.count() << std::endl; // print 2
std::cout << ptr.count() << std::endl; // print 0
```
 
**Важно**
Интерфейс для `Unique_ptr`
```
pointer release() // Releases the ownership of the managed object
void reset(pointer ptr); // Replaces the managed object
pointer get() // Returns a pointer to the managed object
explicit operator bool() // like get() != nullptr
pointer operator->()
T& operator*()
T& operator[](size_t i) // if Unique_ptr store array
```
 
Интерфейс для `Shared_ptr`
```
void reset(); //Replaces the managed object with an object pointed to by ptr
void reset(pointer); //Replaces the managed object with an object pointed to by ptr
void reset(pointer, Deleter); //Replaces the managed object with an object pointed to by ptr
T* get(); // Returns the stored pointer.
long use_count(); // Returns the number of different shared_ptr instances managing the current object
 
explicit operator bool() // like get() != nullptr
pointer operator->()
T& operator*()
T& operator[](size_t i) // if Unique_ptr store array
```

