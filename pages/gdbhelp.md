## Пример программы factorial.c (с ошибками)
collapsed:: true
	- ```
	  #include <stdio.h>
	  
	  
	  int main(void)
	  {
	  	//factorials arent defined for negative numbers 
	  	int num;
	  	do
	  	{
	  		printf("Enter a positive integer: ");
	  		scanf("%d", &num);
	  	}
	  	while (num < 0);
	  
	  	int factorial;
	  	for(int i = 1; i <= num; i++)
	  		factorial = factorial * i;
	  
	  	printf("%d! = %d/n", num, factorial);
	  
	  	return 0;
	  }
	  ```
- **чтобы адекватно дебажить, в пограмме должны быть символы дебага. Для того, чтобы их туда записать нужно при компиляции объектных файлов указывать флаг -g или -g3 (g3 - больше символов, больше информации)**
- **gdb <executable_file> - запустить gdb, после этого ему можно писать всякие команды**
	- ## General
		- quit/q - выйти из  gdb
		- ctrl+p - предыдующая команда
		- ctrl+n - следующая команда
		- ctrl+a - начало строчки
		- ctrl+e - конец строчки
		- ctrl+f (ctrl+rightArrow) - переместиться вперед на символ
		- ctrl+b (ctrl+leftArrow) - переместиться назад на символ
		- run/r - запустить программу или перезапусить ее прямо в процессе дебагинга
		- next/n - выполнить строчку на которой остановился и перейти к следующей строке (то есть будет выполнена строчка, которую gdb сейчас показывает, затем он покажет следующую строчку)
		- list/l - вывести на экран часть кода, посередине которого будет строчка на который мы в данный момент остановились. Повторное введение команды выведет код дальше и так далее
		- list/l <number_of_line> - вывести на экран часть кода вокруг конкретной строки
		- <enter> - повторить предыдущую команду gdb
		- continue/c - переместиться к следующей breakpoit
		- В gdb можно использовать make
		- info
			- breakpoints
			- locals - выводит на экран все локальные переменные
			- registers - содержимое регистров
			- args - вывести аргументы текущей функции
			- frame - вывести информацию о текущем stackframe
	- ## breakpoints
		- break/b <function_name>- установить breakpoint то есть точку, где дебагер будет останавливаться
		  collapsed:: true
			- После каждой breakpoint будет небольшая информация
			- Breakpoint <number>, <function_name> at <file_name>:<line,that is about to be done>
			  <number of line>                     <line about to execute but has not yet>
			  **В нашем примере:**
			- ```
			  (gdb) b main
			  Breakpoint 1 at 0x1195: file factorial.c, line 5.
			  (gdb) r
			  Starting program: /home/andrew/MasterFolder/test/factorial 
			  [Thread debugging using libthread_db enabled]
			  Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
			  Breakpoint 1, main () at factorial.c:5
			  5	{
			  (gdb) next
			  10			printf("Enter a positive integer: ");
			  (gdb)
			  ```
		- break/b <file_name>:<line_number> - установить breakpoint на строчке в файле
		- disable/delete - удалить все breakpoints
		- delete <id_number_of_breakpoint> - удалить конкрутную breakpoint
	- ## variables
		- print/p <name> - вывести на экран переменную с именем name.
		  collapsed:: true
			- **Например**
			- ```
			  gdb) n
			  16		for(int i = 1; i <= num; i++)
			  (gdb) p num
			  $1 = 4
			  (gdb) 
			  
			  ```
			- $1 - специальная переменная gdb, которая теперь хранит значение 4, их тоже можно вызывать
		- print/p <name> = value - буквально присвоить значение какой то переменной, и код будет дальше выполняться с этим значением этой переменной
		- display <name> - при любой команде будет выводиться на экран значение переменной <name>
		- undisplay <id_numder> - отменяет соответствуюющий display.
		  collapsed:: true
			- id_number - номер под которым переменная выводилась на экран в display
			  <id_number>: <name> = <value>
			  Например
			  ```
			  (gdb) display i
			  2: i = (int *) 0x7fffffffe874
			  (gdb) undisplay 2
			  ```
			  Эти команды для постоянного вывода и отмены постоянного вывода i. В этом случае id у i был 2.
		- watch <name> - watches variable <name>
		  collapsed:: true
			- Программа будет останавливаться, если значение переменной изменяется и выдавать некоторую информацию об изменениях
			- Например:
			  ```
			  (gdb) watch x
			  Hardware watchpoint 2: x
			  (gdb) c
			  Continuing
			  
			  Hardware watchpoint 2: x
			  
			  Old value = 10
			  New value = 30
			  bar (p=@0x7fffffffe874: 30) at example.cc:11
			  11                return unknown(p);
			  ```
			  Здесь программа остановилась, когда значение х изменилось с 10 до 30 в результате работы функции bar, в которую передали значение 30 (видимо по ссылке раз там адрес, но это может быть также адрес локальной переменной функции), в строчке 11 файла example.cc, а следующая строчка будет return unknown(p);
		- whatis/what <var_name> - узнать тип переменной
		- set var <name> = <value> - присвоить переменной значени
	- ## stack
		- up/down - двигаться вверх и вниз по call stack. То есть заходить внутрь функций (down), которые мы вызываем и смотреть что вызвало в какой строчке функцию, в которой сейчас находимся (up)
		- backtrace/bt- распечатать весь call stack, чтобы видеть последовательность вызова функций
		- step - войти в функцию, чтобы посмотреть ее код и как он выполняется
		- advance <func_name> - зайти в конкретную функцию
	- ## advanced
		- target record-full - записывать все действия в программе для того, чтобы можно было откатиться например на строчку назад
		- reverse-next/r-n - откатиться на строчку назад
			- также существуют reverse-step и reverse-continue
		- ctrl-x-a    - ахуенный ideшный режим