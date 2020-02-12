## Релиазовать вектор и класс адаптер стек
Необходимо реализовать шаблонный класс вектор со следующим интерфейсом
```
//constructor
Vector(size_t size);
Vector(std::initializer_list<T>);
Vector(T* begin, T* end);
Vector(const Vector&);
~Vector();
Vector& operator=(const Vector&);

// methods
T* begin();
const T* end();
void push_back(T value);
void push_front(T value);
// этот метод являеться опциональным
template<typename T1>
void emplace_back(T1 value)

T* insert(T value);
T *erase(size_t pos);
T *erase(int *pos);
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

Важно в этом классе это реализовать политику перераспределения памяти. Т.е. когда `capacity` вектора равняется `size` элементов, вам необходимо выделить новую память в размере `capacity * 2`.
Потом необходимо реализовать класс `Stack` который будет адаптером, т.е. унаследован `private` от класса `Vector` и реализует только необходимый интерфейс. Но необходимо быть внимательным в плане конструкторов, и операторов присвоения базового класса.
Необходимо подумать для каких методов необходимо написать аналоги константные методы. 

### Важно
Класс `Vector` и класс `Stack` необходимо реализовать как шаблонный класс.