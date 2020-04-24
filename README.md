## Релиазовать вектор и класс адаптер стек
Необходимо реализовать шаблонный класс вектор со следующим интерфейсом
```
//constructor
Vector(size_t size);
template<class T1>
Vector(std::initializer_list<T1>);

template<class T1>
Vector(T1* begin, T1* end);

Vector(const Vector&);
~Vector();
Vector& operator=(const Vector&);

// methods
T* begin();
const T* end();
void push_back(T value);
void push_front(T value);

template<typename T1>
void emplace_back(T1 value) // подумайте как реализовать этот метод(place in)

T* insert(T value);
T *erase(size_t pos);
T *erase(T *pos);
T *erase(T *begin, T* end);

T back();
T front();
T& opearotor[](size_t pos);
const T& opearotor[](size_t pos) const;
void resize(size_t count);
void reserve(size_t new_cap);
size_t size();
size_t capacity();
bool empty();

```

### Важно
Класс `Vector` и класс `Stack` необходимо реализовать как шаблонный класс.

Важно в этом классе это реализовать политику перераспределения памяти. Т.е. когда `capacity` вектора равняется `size` элементов, вам необходимо выделить новую память в размере `capacity * 2`.
Потом необходимо реализовать класс `Stack` который будет адаптером. Класс `Stack` являеться адаптером для класса который будет переден как параметр шаблонна. Т.е. он использует объект адаптер как композиция. Класс  `Stack` он не имеет состояния и является лишь прокси обьектом. Класс  `Stack`  реализовывает интерфейс который свойствен стеку ( LIFO)

Например
```cpp
template<class T>
class Vector {
  ....
};

template<class T, class Cont>
class Stack{
  ...
  private:
    Cont<T> proxy;
}
```
