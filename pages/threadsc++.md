- ```c++
  #include <thread>
  ```
- this_thread::get_id() - получить id потока
- ```c++
  #include <chrono> // для работы со временем
  ```
  this_thread::sleep_for(chrono::milliseconds(3000)) - остановить поток на некоторое время
- Получить максимальное количество потоков, доступное процессору:
  
  ```c++
  int threadsNumber = std::thread::hardware_concurrency();
  ```
- # Работа с потоками
  collapsed:: true
	- thread <thread_name>(<func_pointer>, <func_param1>,<func_param2>, ....); - уже создастся отдельный поток и этот метод будет выполнятся в отдельном потоке
	- <thread_name>.detach(); - разрыв связи между <thread_name> и потоком где он создавался (условно если закончится поток main, в котором был объявлен thread th, также закончится и поток th - жесть undefined behavior)
	- <thread_name>.join(); - нужно вызывать в том месте кода, где мы хотим дождаться выполнения <thread_name>. При этом поток <thread_name> завершится даже если завершился поток, в котором он был объявлен
	- <thread_name>.~thread - деструктор потока
	- Чтобы передать в поток переменную по ссылке нужно использовать std::ref(<var>)
	- Передача методов класса
	  
	  thread t(&<Class_Name>::<method_name>, <class_object>);
	  
	  Пример:
	  ```c++
	  class MyClass{
	         void func(){}
	  };
	  
	  int main(){
	        MyClass m;
	        thread t(&MyClass::func, m);
	        return 0;
	  }
	  ```
- # Mutex
  collapsed:: true
	- ```c++
	  #include <mutex>
	  ```
	- Объявляем объект класса mutex
	  ```c++
	  mutex mtx;
	  ```
	- Участок кода, который мы ходим оградить заключаем в mtx.lock() и mtx.unlock()
	  
	  ```c++
	  void func(){
	        mtx.lock();
	        // some code
	        mtx.unlock();
	  }
	  ```
	- ## Lock_guard
	  collapsed:: true
		- Задача lock_guard - захватить mutex в конструкторе (своем) и отпустить в деструкторе
		  Т.е при создании объекта lock_guard происходит mutex.lock(), при разрушении объекта происходит mutex.unlock()
		- Синтаксис - lock_guard<mutex> <lock_guard_obj_name>(<mutex_name>);
		- ```c++
		  void func(){
		           lock_guard<mutex> guard(mtx); // mtx.lock()
		           //some code
		  } // mtx unlocks when function ends (where guard destroyed)
		  ```
	- recursive mutex - его можно блокировать несколько раз, но и разблокировать его надо столько же раз. Используется в рекурсивных функциях
	  
	  ```c++
	  recursive_mutex rmtx;
	  ```