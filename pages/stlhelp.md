# Vector
	- ## Память
		- .size()
		- .capacity()
		- .shrink_to_fit() - урезать capacity до size
		- .reserve(i) - зарезервировать память(установить capacity) на i элементов
	- ## Управление элементами
		- .push_back(el) - добавить в конец
		  .pop_back() - удаление последнего элемента
		  .at(i) ~ [i], но .at проверяет границы массива, но работает медленнее - то есть для лохов
		  .clear() - удаляет все элементы
		  .empty() - провека есть ли элементы
	- ## Работа с итераторами
		- .insert(iteratorName, element) - вставка элемента на место iterator
		  .erase(iteratorName) - удаление элемента по итератору
		  .erase(startIterator, endIterator) - удаляет несколько элементов со start до (end - 1) включительно по итератору
- # Iterator
  collapsed:: true
	- vector<type>::iterator iteratorName - объявление итератора, который по сути является умным указателем
	  .begin() - возвращает итератор начала
	  .end() - итератор конца, ~ '\0'
	  advance(iteratorName, i) - смещение итератора на i
- # insert_iterator / inserter
  collapsed:: true
	- std::insert_iterator<Container> <name>(CONSTRUCTOR)
	- В конструкторе надо указать два аргумента: контейнер и начальный итератор, с которого начнется вставка
	- Объявляет итератор, по которому можно вставлять значения в контейнер. Вставка осуществляется просто присвоением итератору некоего значения. После вставки итератор инкрементируется на 1
	- ```c++
	  std::vector<int> v = {1,2,3};
	  std::insert_iterator<std::vector<int>> it(v, v.begin());
	  it = 0;
	  std::cout << v; // 0,1,2,3
	  it = 500;
	  std::cout << v; //0, 500, 1, 2, 3
	  it = 300;
	  std::cout << v; //0,500,300,1,2,3
	  ```
	- ```c++
	  std::map<int,int> mp = {{1,2},{3,4}};
	  std::insert_iterator<std::map<int,int>> it(mp, mp.begin());
	  it = {0,0}; //mp = {0,0} {1,2} {3,4}
	  it = {100,100}; //mp = {0,0} {100,100} {1,2} {3,4}
	  ```
	- insert_iterator нельзя просто так инкрементровать или разименовать
	- inserter - удобный конструктор для insert_iterator, принимает на вход контейнер и итератор контейнера, возвращает inster_iterator
	- Можно его воспринимать как
	  ```c++
	  template<class Container>
	  std::insert_iterator<Container> inserter(Container& c, typename Container::iterator i)
	  {
	      return std::insert_iterator<Container>(c, i);
	  }
	  ```
- # List (двусвязный)
  collapsed:: true
	- .push_back(el) - в конец
	  .push_front(el) - в начало
	  .pop_back()
	  .pop_front()
	  .clear()
	  .unique() - удаляет последовательно идущие дубликаты (то есть если использовать вместе с sort, то все удалит дубликаты)
	  .remove(el) - удаление элементов по значению
	  .sort() - от наименьшего к наибольшему
	  .insert(iterator, el)
	  .erase(iterator)
	  .assign(кол-во элементов, el) - заполняет лист элементами el в заданном количествe
	  
	  копирование значений одного листа в другой ~strcpy:
	  list.assign(list2.begin(), list2.end()); - копирование list2 в list
	  
	  forward_list - односвязный список, такие же методы +-
- # Map
  collapsed:: true
	- .emplace(key, value)
	  .insert(std::pair)
	  .find(key) - возвращает итератор, если смог найти, если не смог - возвращает end
	  [key] - возвращает значение
	  .at(key) = value - попытка присвоить значение, если такого key нет будет исключение
	  .erase(key)
- # Set
  collapsed:: true
	- .insert(el) - добавить элемент
	  .find(el) - возвращает итератор на элемент, если нашел, end() - если не нашел
	  .erase(value) - удаление по значению, но не по итератору
- # Multiset
  collapsed:: true
	- .lower_bound(el) - возвращает итератор первого входящего элемента el
	  .upper_bound(el) - возвращает итератор элемента, находящегося после последнего элемента el
	  .equal_range(el) - возвращает и lower_bound и uppper_bound
- # Deque
  collapsed:: true
	- .push_back(el)
	  .push_front(el)
	  .at(index)
	  .insert()
- # other stuff
	- Можно посчитать количество элементов в векторе (возможно не только в векторе) с каким то условием
	  std::count_if(<left_brdr>, <right_brdr>, comparator);
	  
	  Пример на векторе:
	  ```c++
	  std::vector<int> v(1000);
	  int x = std::count_if(v.begin(), v.end(), [](int i){return i == 0;});
	  // Посчитали количество нулей
	  ```