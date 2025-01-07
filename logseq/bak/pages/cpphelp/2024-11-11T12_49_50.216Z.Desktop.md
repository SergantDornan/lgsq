# Cast
collapsed:: true
	- **1. `static_cast`**
	  
	  * **Используется для:**  Преобразований между типами, которые связаны между собой (например, `int` и `float`), и преобразований, которые разрешены компилятором (например, преобразование базового класса к производному).
	  * **Безопасность:** Относительно безопасный, так как компилятор проверяет, что преобразование допустимо. 
	  * **Пример:** `int i = static_cast<int>(3.14);`
	  
	  **2. `const_cast`**
	  
	  * **Используется для:** Изменения квалификатора `const` переменной. 
	  * **Безопасность:** Относительно безопасный, но требует осторожности, так как может привести к непредсказуемому поведению, если вы попытаетесь изменить значение объекта, который должен быть `const`.
	  * **Пример:** `const int *ptr = new int(5); int *nonConstPtr = const_cast<int*>(ptr);`
	  
	  **3. `dynamic_cast`**
	  
	  * **Используется для:** Преобразований между типами, которые связаны полиморфно (т.е. имеют общий базовый класс).
	  * **Безопасность:** Наиболее безопасный, так как компилятор проверяет, что преобразование допустимо во время выполнения. 
	  * **Пример:**  
	  ```c++
	  class Base { };
	  class Derived : public Base { };
	  
	  Base *base = new Derived(); 
	  Derived *derived = dynamic_cast<Derived*>(base);
	  ```
	  
	  **4. `reinterpret_cast`**
	  
	  * **Используется для:** Преобразования между типами, которые **не связаны** никаким образом. 
	  * **Безопасность:** Наиболее опасный, так как **не гарантирует** корректность преобразования.  Компилятор **не проверяет** допустимость преобразования. 
	  * **Пример:** `int *ptr = reinterpret_cast<int*>(0x12345678);`
- # Conversions
	- Конвертация string в числовые типы
	  
	  
	  *std::stoi, std::stol, std::stoll: для конвертации в int, long, long long соответственно.
	  * std::stod, std::stof, std::stold: для конвертации в double, float, long double соответственно.
	  * std::stoul, std::stoull: для конвертации в unsigned long, unsigned long long соответственно.
	  * std::stoi: для конвертации в size_t.
	- ## std::stringstream
		- ```c++
		  #include <sstream>
		  ```
		- С помощью std::stringstream можно конвертировать много что во много что
		- Засовываешь в поток что угодно и можешь оттуда достать string, int или любой другой тип
		- ```c++
		  std::stringstream stream;
		  stream << s; // Засунули в поток string s
		  int x;
		  stream >> x; // Достали int, конвертировали из string
		  double y;
		  stream >> y; // Достали double
		  ```
		- ```c++
		  std::stringstream stream;
		  int x = 10;
		  stream << x; // Засунули int
		  std::string s;
		  stream >> s; // Достали string
		  ```
		- Для stringstream точно так же работают перегрузки << и >>
- # functions
  collapsed:: true
	- ## func pointers
	  collapsed:: true
		- Указатели на функции:
		  тип (*имя_указателя) (типы_параметров);
		- ```
		  void hello();
		  int main(){
		       void (*message)(); //объявление
		       message = hello;
		       message();
		  }
		  ```
	- ## Lambda
	  collapsed:: true
		- ```c++
		  auto F = [&solution, &power](long double xi) -> long double{
		  		long double result = 0;
		  		for(int i = 0; i < power + 1; ++i){
		  			result += solution[i] * pow(xi, power - i);
		  		}
		  		return result;
		  	};
		  ```
		- [] - захват переменных извне, иначе нельзя будет использовать локальные переменные внутри лямбда функции, надо захватывать
		  
		  [&] - захватить все что есть по ссылке
		- (long double xi) - аргументы функции
		- -> long double  - возвращаемое значение
- # Comparator
  collapsed:: true
	- ```c++
	  auto comp = [](T x, T y){return pr(x) < pr(y)}; //Обязательно оператор <
	  std::sort(begin,end,comp);
	  ```
- # other stuff
	- std::getline(std::cin, str, ','); - str это std::string, третий аргумент - разделитель до которого будет взята строка, если третий аргумент не писать, то будет взята строка полностью
	  std::cin.getline(char*, 256, ',');
	- ```c++
	  #include <typeinfo>
	  ```
	  typeid(<var_name>) - информация о типе переменной
	  typeid(<var_name>).name() - имя типа переменной