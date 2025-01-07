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
  collapsed:: true
	- Описание всех папок, их названий и что туда надо складывать, чтобы make работал
	- ## algAnal
	  collapsed:: true
		- здесь лежит код анализатора сложности алгоритма
		- ### header
		- ### source
	- ## ContestCode
	  collapsed:: true
		- Лежит код для генерации файла, который надо вставить в контестик
		- ### header
		- ### source
		- Лежит сам файлик, содержимое которого надо вставить
	- ## depsAndObject (наличие обязательно)
	  collapsed:: true
		- Наполняется автоматически, здесь хранятся файлы зависимостей make и также объектные файлы
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
			- .cpp файлы для кода динамических библиотек.
	- ## staticLibs
	  collapsed:: true
		- здесь хранятся статические библиотеки и автоматически складываются статические библиотеки, которые создаем сами
	- ## staticLibsSource
	  collapsed:: true
		- ### header
		  collapsed:: true
			- .h файлы для кода статических библиотек
		- ### source
		  collapsed:: true
			- .cpp файлы для кода статических библиотек.
	- ## tests
	  collapsed:: true
		- здесь лежит код unit тестов
		- ### header
		- ### source
	- main.cpp
	  Makefile
	  можно другие файлы .h и .cpp
- # Universal Makefile (для контестов)
	- Чтобы все работало надо построить проект по Project structure выше или заменить названия папок в Makefile. Можно не создавать все папки и не класть все указанные файлы в папки, make сам разберется что у тебя есть и чего нет и все соберет как надо
	  
	  !!!! Перед использованием Makefile ОЧЕНЬ рекомендуется пролистать ВСЕ что есть в projectstructure.
	- !!! Также в main.cpp изменена точка входа (по умолчанию mainfunc). Если нужно вернуть как было (не рекомендуется), то в makefile изменить переменную MAINENTRY обратно на main или как удобно
	- ## Команды make:
	  collapsed:: true
		- all - собрать основную программу, unit тесты и анализатор кода
		- mrproper - удаление всех файлов, которые мог создавать make, возвращение к иходному коду
		- anal - сборка анализатора алгоритма
		- tst - сборка unit тестов
		- prog - сборка основной программы
		- libs - собрать все библиотеки
		- libstatic - собрать статические библиотеки
		- libshared - собрать динамические библиотеки
		- run - собрать и запустить основную программу
		- runanal - собрать и запустить анализатор кода
		- runtst - собрать и запустить unit тесты
		- debug - запустить основную программу в gdb
		- debuganal - запустить анализатор в gdb
		- debugtst - запустить юнит тесты в gdb
		- contestcode - сгенерировать файл с кодом, чтобы вставить его в контестик
	- ## Makefile
		- collapsed:: true
		  ```make
		  ANAL=analysis
		  OUTPUT=outprog
		  TST=test
		  
		  ANALENTRY=entry
		  TSTENTRY=TSTentry
		  MAINENTRY=mainfunc
		  
		  ContCodeDir = ./ContestCode
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  DEPOBJDIR =./depsAndObjects
		  STATLIBS=./staticLibs
		  SHLIBS=./sharedLibs
		  Static_lib_folder_code=./staticLibsSource
		  Shared_lib_folder_code=./sharedLibsSource
		  lib_code_source=/source
		  lib_code_headers=/header
		  
		  SOURCEANAL=./algAnal/source
		  SOURCETST=./tests/source
		  
		  INCANAL=./algAnal/header
		  INCTST=./tests/header
		  
		  ContINC = $(ContCodeDir)/header
		  ContSource = $(ContCodeDir)/source
		  ContOUTPUT = $(ContCodeDir)/filemaker
		  ContFile = $(ContCodeDir)/code.cpp
		  
		  TSTfile = ./tests/testFile
		  ANSfile = ./tests/answerFile
		  
		  INCDIRS=. ./include/ $(INCANAL) $(INCTST) $(ContINC)
		  
		  STATICLIBGEN_name=static
		  SHAREDLIBGEN_name=shared
		  CPPC=g++
		  C++standart=-std=c++23
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=$(C++standart) -g3
		  
		  OUTPUTS=$(OUTPUT) $(ANAL) $(TST) $(ContOUTPUT)
		  SOURCESTATIC=$(Static_lib_folder_code)$(lib_code_source)
		  SOURCESHARED=$(Shared_lib_folder_code)$(lib_code_source)
		  INCDIRSTATIC=$(Static_lib_folder_code)$(lib_code_headers)
		  INCDIRSHARED=$(Shared_lib_folder_code)$(lib_code_headers)
		  STLIBGEN=$(STATLIBS)/lib$(STATICLIBGEN_name).a
		  SHLIBGEN=$(SHLIBS)/lib$(SHAREDLIBGEN_name).so
		  
		  CFLAGS=$(GENERALFLAGS) $(OPT) $(DEPFLAGS)
		  
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  
		  STATICCFILES=$(foreach D, $(SOURCESTATIC), $(wildcard $(D)/*.cpp))
		  SHAREDCFILES=$(foreach D, $(SOURCESHARED), $(wildcard $(D)/*.cpp))
		  
		  OBJECTSSTATIC=$(patsubst $(Static_lib_folder_code)$(lib_code_source)%.cpp, $(DEPOBJDIR)%.o, $(STATICCFILES))
		  OBJECTSSHARED=$(patsubst $(Shared_lib_folder_code)$(lib_code_source)%.cpp, $(DEPOBJDIR)%.o, $(SHAREDCFILES))
		  
		  CONTCFILES=$(foreach D, $(ContSource), $(wildcard $(D)/*.cpp))
		  CONTOBJECTS=$(patsubst $(ContSource)%.cpp, $(DEPOBJDIR)%.o, $(CONTCFILES))
		  
		  OBJECTS=$(patsubst $(ROOTDIR)%.cpp, $(DEPOBJDIR)%.o, $(CFILESROOT)) $(patsubst $(SOURCEDIR)%.cpp, $(DEPOBJDIR)%.o, $(CFILESSOURCE))
		  DEPFILES=$(patsubst $(ContSource)%.cpp, $(DEPOBJDIR)%.d, $(CONTCFILES)) $(patsubst $(ROOTDIR)%.cpp, $(DEPOBJDIR)%.d, $(CFILESROOT)) $(patsubst $(SOURCEDIR)%.cpp, $(DEPOBJDIR)%.d, $(CFILESSOURCE)) $(patsubst $(SOURCESTATIC)/%.cpp, $(DEPOBJDIR)/%.d, $(STATICCFILES)) $(patsubst $(SOURCESHARED)/%.cpp, $(DEPOBJDIR)/%.d, $(SHAREDCFILES)) $(patsubst $(SOURCEANAL)/%.cpp, $(DEPOBJDIR)/%.d, $(ANALCFILES)) $(patsubst $(SOURCETST)/%.cpp, $(DEPOBJDIR)/%.d, $(TSTCFILES))  
		  LIBSTATIC_files=$(foreach D, $(STATLIBS), $(wildcard $(D)/lib*.a))
		  LIBSHARED_files=$(foreach D, $(SHLIBS), $(wildcard $(D)/lib*.so))
		  LIBSTATIC_names=
		  LIBSHARED_names=
		  
		  ifneq ($(LIBSHARED_files), )
		  	LIBSHARED_names:=$(patsubst $(SHLIBS)/lib%.so, %, $(LIBSHARED_files))
		  else
		  	LIBSHARED_names:=
		  endif
		  
		  ifneq ($(LIBSTATIC_files), )
		  	LIBSTATIC_names:=$(patsubst $(STATLIBS)/lib%.a, %, $(LIBSTATIC_files))
		  else
		  	LIBSTATIC_names:=
		  endif
		  
		  
		  
		  CONTdepend=
		  
		  ifneq ($(strip $(CONTCFILES)), )
		  	CONTdepend:=$(ContOUTPUT)
		  else
		  	CONTdepend:=
		  endif
		  
		  ANALCFILES=$(foreach D, $(SOURCEANAL), $(wildcard $(D)/*.cpp))
		  TSTCFILES=$(foreach D, $(SOURCETST), $(wildcard $(D)/*.cpp))
		  ANALOBJECTS=$(patsubst $(SOURCEANAL)%.cpp, $(DEPOBJDIR)%.o, $(ANALCFILES))
		  TSTOBJECTS=$(patsubst $(SOURCETST)%.cpp, $(DEPOBJDIR)%.o, $(TSTCFILES))
		  
		  ANALdepend=
		  STATICdepend=
		  SHAREDdepend=
		  TSTdepend=
		  
		  ifneq ($(strip $(STATICCFILES)), )
		  	STATICdepend:=$(STLIBGEN)
		  else
		  	STATICdepend:=
		  endif
		  
		  ifneq ($(strip $(SHAREDCFILES)), )
		  	SHAREDdepend:=$(SHLIBGEN)
		  else
		  	SHAREDdepend:=
		  endif
		  
		  ifneq ($(strip $(ANALCFILES)), )
		  	ANALdepend:=$(ANAL)
		  else
		  	ANALdepend:=
		  endif
		  
		  ifneq ($(strip $(TSTCFILES)), )
		  	TSTdepend:=$(TST)
		  else
		  	TSTdepend:=
		  endif
		  
		  STATICLIBGEN_link:=
		  SHAREDLIBGEN_link:=
		  
		  ifneq ($(strip $(STATICCFILES)), )
		  	STATICLIBGEN_link:=-l$(STATICLIBGEN_name)
		  else
		  	STATICLIBGEN_link:=
		  endif
		  
		  ifneq ($(strip $(SHAREDCFILES)), )
		  	SHAREDLIBGEN_link:=-l$(SHAREDLIBGEN_name)
		  else
		  	SHAREDLIBGEN_link:=
		  endif
		  
		  ISSHARED=
		  ISSTATIC=
		  
		  ifneq ($(strip $(SHAREDCFILES)), )
		  	ISSHARED:=$(foreach D,$(SHLIBS),-L$(D))
		  else
		  	ISSHARED:=$(ISSHARED)
		  endif
		  
		  ifneq ($(strip $(LIBSHARED_files)), )
		  	ISSHARED:=$(foreach D,$(SHLIBS),-L$(D))
		  else
		  	ISSHARED:=$(ISSHARED)
		  endif
		  
		  ifneq ($(strip $(STATICCFILES)), )
		  	ISSTATIC:=$(foreach D,$(STATLIBS),-L$(D))
		  else
		  	ISSTATIC:=$(ISSTATIC)
		  endif
		  
		  ifneq ($(strip $(LIBSTATIC_files)), )
		  	ISSTATIC:=$(foreach D,$(STATLIBS),-L$(D))
		  else
		  	ISSTATIC:=$(ISSTATIC)
		  endif
		  
		  INCLUDESHARED:=
		  
		  ifeq ($(ISSHARED),$(foreach D,$(SHLIBS),-L$(D)))
		  	INCLUDESHARED:=export LD_LIBRARY_PATH=$$LD_LIBRARY_PATH:$(SHLIBS)
		  else
		  	INCLUDESHARED:=
		  endif
		  
		  all:$(OUTPUT) $(ANALdepend) $(TSTdepend) $(CONTdepend)
		  	@echo ========= SUCCES ==========
		  
		  run:$(OUTPUT)
		  	@./$(OUTPUT)
		  
		  runanal:$(ANALdepend)
		  	@./$(ANAL) output
		  
		  runtst:$(TSTdepend)
		  	@./$(TST)
		  
		  debug:$(OUTPUT)
		  	@gdb ./$(OUTPUT)
		  
		  debuganal:$(ANALdepend)
		  	@gdb ./$(ANAL)
		  
		  debugtst:$(TSTdepend)
		  	@gdb ./$(TST)
		  
		  anal:$(ANALdepend)
		  
		  libs:$(STATICdepend) $(SHAREDdepend)
		  
		  libstatic:$(STATICdepend)
		  
		  libshared:$(SHAREDdepend)
		  
		  tst:$(TSTdepend)
		  
		  prog:$(OUTPUT)
		  
		  contestcode:$(CONTdepend)
		  	@$(CONTdepend)
		  	@#sublime-text.subl $(ContFile)
		  
		  $(OUTPUT):$(STATICdepend) $(SHAREDdepend) $(OBJECTS)
		  	$(CPPC) $^ -Wl,--defsym=main=$(MAINENTRY) $(ISSTATIC) $(ISSHARED) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) $(STATICLIBGEN_link) $(SHAREDLIBGEN_link) -o $@
		  	$(INCLUDESHARED)
		  
		  $(ANAL):$(STATICdepend) $(SHAREDdepend) $(ANALOBJECTS) $(OBJECTS)
		  	$(CPPC) $(OBJECTS) $(ANALOBJECTS) -Wl,--defsym=main=$(ANALENTRY) $(ISSTATIC) $(ISSHARED) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) $(STATICLIBGEN_link) $(SHAREDLIBGEN_link) -o $@
		  	$(INCLUDESHARED)
		  
		  $(TST):$(ANSfile) $(TSTfile) $(STATICdepend) $(SHAREDdepend) $(TSTOBJECTS) $(OBJECTS) $(ANALOBJECTS)
		  	$(CPPC) $(OBJECTS) $(TSTOBJECTS) $(ANALOBJECTS) -Wl,--defsym=main=$(TSTENTRY) $(ISSTATIC) $(ISSHARED) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) $(STATICLIBGEN_link) $(SHAREDLIBGEN_link) -o $@
		  	$(INCLUDESHARED)
		  
		  $(ContOUTPUT):$(ContFile) $(STATICdepend) $(SHAREDdepend) $(CONTOBJECTS)
		  	$(CPPC) $(CONTOBJECTS) $(ISSTATIC) $(ISSHARED) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) $(STATICLIBGEN_link) $(SHAREDLIBGEN_link) -o $@
		  	$(INCLUDESHARED)
		  
		  $(ContFile):
		  	touch $(ContFile)
		  
		  $(TSTfile):
		  	touch $(TSTfile)
		  
		  $(ANSfile):
		  	touch $(ANSfile)
		  
		  mrproper:
		  	rm -rf $(OUTPUTS) $(OBJECTS) $(DEPFILES) $(STLIBGEN) $(SHLIBGEN) $(OBJECTSSTATIC) $(OBJECTSSHARED) $(ANALOBJECTS) $(TSTOBJECTS) $(CONTOBJECTS) $(ContFile) $(TSTfile) $(ANSfile)
		  
		  $(STLIBGEN):$(OBJECTSSTATIC)
		  	ar rc $(STLIBGEN) $(OBJECTSSTATIC)
		  	ranlib $(STLIBGEN)
		  
		  $(SHLIBGEN):$(OBJECTSSHARED)
		  	$(CPPC) -shared -o $(SHLIBGEN) $(OBJECTSSHARED)
		  
		  $(DEPOBJDIR)/%.o:$(SOURCESHARED)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRSHARED),-I$(D)) -fPIC -c $< -o $@
		  
		  $(DEPOBJDIR)/%.o:$(SOURCESTATIC)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRSTATIC),-I$(D)) -c $< -o $@
		  
		  $(DEPOBJDIR)/%.o:$(ROOTDIR)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRS),-I$(D)) -c -o $@ $<
		  
		  $(DEPOBJDIR)/%.o:$(SOURCEDIR)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRS),-I$(D)) -c -o $@ $<
		  
		  $(DEPOBJDIR)/%.o:$(SOURCEANAL)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRS),-I$(D)) -c -o $@ $<
		  
		  $(DEPOBJDIR)/%.o:$(SOURCETST)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRS),-I$(D)) -c -o $@ $<
		  
		  $(DEPOBJDIR)/%.o:$(ContSource)/%.cpp
		  	$(CPPC) $(CFLAGS) $(foreach D,$(INCDIRS),-I$(D)) -c -o $@ $<
		  
		  -include $(DEPFILES) 
		  ```
			- file:///home/andrew/MasterFolder/f.txt