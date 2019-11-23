## Релиазовать вектор и класс адаптер стек
Необходимо реализовать класс вектор со следующим интерфейсом
```
//constructor
Vector(size_t size);
Vector(std::initializer_list<int>);
Vector(int*begin, int* end);
Vector(const Vector&);
~Vector();

// methods
int* begin();
const int* end();
void push_back(int value);
void push_front(int value);
int* insert(int value);
int *erase(size_t pos);
int *erase(int *pos);
int *erase(int *begin, int* end);

int back();
int front();
void resize(size_t count);
size_t size();
size_t capacity();
bool empty();

```

Важно в этом классе это реализовать политику перераспределения памяти. Т.е. когда `capacity` вектора равняется `size` элементов, вам необходимо выделить новую память в размере `capacity * 2`.
Также необходимо добавить const в некоторые методы.
Потом необходимо реализовать класс `Stack` который будет адаптером, т.е. унаследован `private` от класса `Vector` и реализует только необходимый интерфейс. Но необходимо быть внимательным в плане конструкторов, и операторов присвоения базового класса, т.е. под объект базового класса.


### Необьязательно
Класс `Vector` и класс `Stack` можно реализовать как шаблонный класс.