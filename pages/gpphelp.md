# Компиляция
collapsed:: true
	- cpp <file_name> > <new_file_name> - выполнить исключительно препроцессинг на файле
	  
	  g++ -S <file_name> - скомпилировать файл, получится файл С АССЕМБЛЕРНЫМ КОДОМ с таким же именем но с расиширением .S
	  
	  as -o <new_file_name> <file_name> - ассемблироание - создание из ассемблерного файла объктног
	  
	  Линковка:
	  !!!! не делай этого пусть g++ сам разбирается
	  ld -e main -o <new_file_name> <file_name> <linked_librarys> - линковка в готовый выходной файл
	  ldd <file_name> - проверить слинкованные библиотеки
	  
	  
	  Полная компиляция:
	  g++ <flags> -o <new_file_name> <all_files_needed>
	  
	  g++ -v <file_name.cpp> - компилятор в режиме расширенного вывода (например можно посмотреть все используемые библиотеки)
- # Изменение точки входа
	- Это нечто работает чисто только с С, чтобы работало в С++ нужно поменять точку входа как extern "C"
	- **!!!!!!! main из main.cpp НЕЛЬЗЯ вызывать вообще ниоткуда, иначе будет SEGFAULT (хз почему) **
	- Пусть есть файлы
	- algs.cpp
	  ```c++
	  #include <iostream>
	  extern void func1();
	  int foo();
	  extern "C" int entry(int argc, char* argv[]){
	  	std::cout << atoi(argv[1]) << std::endl;
	  	std::cout << "AAAAA" << std::endl;
	  	foo();
	      func1();
	  	return 0;
	  }
	  ```
	- main.cpp
	  ```c++
	  #include <iostream>
	  extern void func1();
	  int foo(){
	    std::cout << "FOO" << std::endl;
	  }
	  int main(){
	  	std::cout << "1000" << std::endl;
	  	std::cout << "MAIN" << std::endl;
	      func1();
	  	return 0;
	  }
	  ```
	- И до кучи, чтобы жизнь малиной не казалась st.cpp
	  ```c++
	  #include <iostream>
	  extern void func1(){
	  	std::cout << "1" << std::endl;
	  }
	  ```
	- Задача - собрать проект так, чтобы точка входа была в algs.cpp с применением статической библиотеки, полученной из st.cpp, используемой в main.cpp и чтобы все работало
	- Решение:
	- ```bash
	  g++ -c -o st.o st.cpp
	  ar rc libLIB1.a st.o 
	  ranlib libLIB1.a #собрали библиотеку
	  g++ -c -o main.o main.cpp #скомпилировали main, библиотеку подрубать не надо
	  g++ -c -o algs.o algs.cpp #скомпилировали algs, библиотеку также подрубать не надо
	  g++ main.o algs.o -Wl,--defsym=main=entry -L. -lLIB1 -o prog
	  ```
	  Последняя команда - сборка из объектых файлов, подключение библиотеки + через флаг -Wl указания линковщику изменить точку входа --defsym=main=<entry_point_name>
- # Флаги
	- ## Оптимизация
		- -O0: Отключение оптимизации (по умолчанию).
		   -O1: Базовая оптимизация, разумный баланс скорости и размера.
		   -O2: Более агрессивная оптимизация, рекомендуется для большинства случаев.
		   -O3: Максимальная оптимизация, может увеличивать время компиляции.
		   -Os: Оптимизация для размера кода.
		   -Ofast: Очень агрессивная оптимизация, может нарушать стандарты C++.
		   -g : добавка отладочных символов (по сути включает в скомпилированный файл целый исходный файл, чтобы его можно было посмотреть) Нужно для дебагинга
		  -g3 : жесть добавка отладочных символов
		   -march=native:  Использовать набор инструкций для текущего процессора.
		   -mtune=native:  Использовать оптимальные параметры для текущего процессора.
	- ## Управление выводом
	  collapsed:: true
		- -c: Компиляция в объектный файл (.o).
		   -o <имя_файла>: Установка имени выходного файла.
		   -Wall: Включение всех предупреждений.
		   -Wextra: Включение дополнительных предупреждений.
		   -pedantic: Проверка кода на соответствие стандарту C++.
		   -Werror: Превращение предупреждений в ошибки.
		   -g3: Включение отладочной информации (символов, для использования gdb).
	- ##  Управление зависимостями
	  collapsed:: true
		- -I <путь>: Добавление пути к директориям с заголовочными файлами.
		   -L <путь>: Добавление пути к директориям с библиотеками.
		   -l <имя_библиотеки>: Линковка статической или динамической библиотеки.
	- ##  Другие флаги
	  collapsed:: true
		- -static:  Статическая линковка всех зависимостей.
		   -shared:  Создание динамической библиотеки.
		   -fPIC:  Создание позитивно-независимого кода (PIC), необходимо для компиляции объектных файлов динамических библиотек.
		   -pthread:  Включение поддержки многопоточности.
		   -D <макрос>:  Определение макроса во время компиляции.
		   -std=c++11/c++14/c++17/c++20:  Выбор стандарта C++.
		   -fno-exceptions:  Отключение поддержки исключений.
		   -fno-rtti:  Отключение поддержки RTTI.
	- ## Безопасность
	  :LOGBOOK:
	  CLOCK: [2024-09-06 Fri 23:43:10]--[2024-09-06 Fri 23:43:11] =>  00:00:01
	  :END:
		- -fno-pic - отключает позиционно-независимый код при компиляции в объектный файл
		- -no-pie - отключает позиционно независимую линковку при компоновке исполняемого файла
		- -fno-stack-protector - не размечать области стека как недоступные на запись (когда опция включена, пользователь может изменить адрес возврата из функции в теле самой функции через переполнение буфера)
		- -D_FORTIFY_SOURCE=0 - отключить проверку переполнения буфера канарейкой (после вызова таких функций как memcpy, memset, strcat)
		- -fno-inline-small-functions - не подставлять вместо вызова функций их содержимое
		- -fno-omit-frame-pointer - всегда сохранять указатель на фрейм в маленьких функциях