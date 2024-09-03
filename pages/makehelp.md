# General
collapsed:: true
	- lib directory is for .cpp files
	  include directory is for .h files
	- ## Variables
	  collapsed:: true
		- <var_name> = <var_value> - any variable (gcc, -I (flag), -O (falg), etc..)
		  works as makros
		  $(<variable_name>) - access to variable 
		  **EXMP:**
		  CFILES = main.c source.c algs.c
		  **ENDEXMP**
	- ## Flags
	  collapsed:: true
		- -MP -MD  magic flags that preserve dependencies between headers and source files by generating *.d files.
	- ## Structure
	  collapsed:: true
		- target: dependencies
		  	commands$
		  
		  
		  @ - in commands is target
		  $^ - in commands is dependencies
		  $< - first file in dependencies
		  make -C <dir> - do a make command in other directory <dir>
	- ## Loops
	  collapsed:: true
		- $(foreach <new_variable_name>, <loop list>, <command to create an expression>)
		  
		  EXMP
		  CODEDIRS=. lib
		  CFILES=$(foreach D, $(CODEDIRS), $(wildcard $(D)/*.cpp))
		  exmp creates "./*.cpp ./*cpp ./lib/*.cpp ./lib/*.cpp" - value for CFILES variable
		  ENDEXMP
	- ## General Expressions
	  collapsed:: true
		- % - regular expression, equal *, any amount of any symbols
		  ? - regular expression, one symbol
		  
		  <variable_name>=$(patsubst <regular_expr_1>, <regular_expr_2>, <list>) - convert one list of strings, that match <regular_expr_1> out of the <list> to list of strings that match <regular_expr_2> and writes this new list to variable
		  
		  **EXMP**
		  OBJECTS=$(patsubst %.c,%.o,$(CFILES)) - for exmp CFILES is "x.c y.c z.c", then OBJECTS will be "x.o y.o z.o"
		  **ENDEXMP**
		  
		  <variable_name>=$(wildcard <some_dir>/<regular_expr>) - creates a list of all files that match <regular_expr> in <some_dir>. % DOES NOT WORK IN LOOPS, only * and only wildcard func.
	- ## other stuff
	  collapsed:: true
		- @<terminal_command> - run the command but do not print the command
		- $(<some_text>) - without a variable just output text to terminal
		- **some shit to create tgz file**
		- distribute: clean
		         tar zcvf dist.tgz *
- # Project Structure
	- Описание всех папок, их названий и что туда надо складывать, чтобы make работал
	- ## deps
	  collapsed:: true
		- Наполняется автоматически, здесь хранятся файлы зависимостей make
	- ## include
	  collapsed:: true
		- здесь хранятся все header файлы основного проекта (не библиотеки)
	- ## lib
	  collapsed:: true
		- здесь хранятся .cpp файлы основного проекта (не библиотеки)
	- ## sharedLibs
	  collapsed:: true
		- здесь хранятся динамические библиотеки и автоматически складываются динамические библиотеки, которые мы создаем сами
	- ## sharedLibsSource
	  collapsed:: true
		- ### header
			- .h файлы для кода динамических библиотек
		- ### source
			- .cpp файлы для кода динамических библиотек
	- ## staticLibs
	  collapsed:: true
		- здесь хранятся статические библиотеки и автоматически складываются статические библиотеки, которые создаем сами
	- ## staticLibsSource
	  collapsed:: true
		- ### header
			- .h файлы для кода статических библиотек
		- ### source
			- .cpp файлы для кода статических библиотек
	- main.cpp
	  Makefile
	  можно другие файлы .h и .cpp
- # Universal makefiles