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
		  collapsed:: true
			- .h файлы для кода статических библиотек
		- ### source
		  collapsed:: true
			- .cpp файлы для кода статических библиотек
	- main.cpp
	  Makefile
	  можно другие файлы .h и .cpp
- # Universal makefiles
  collapsed:: true
	- Чтобы все работало надо построить проект по Project structure выше или заменить названия папок в Makefile
	- ### Команды make во всех последующих файлах:
	  mrproper - удаление всех файлов, которые мог создавать make, возвращение к иходному коду
	  
	  libs - только лишь собрать библиотеки собственного написания (если такие имеются)
	- ## Полный пакет (наличие всех видов библиотек, наличие кода к собственным библиотекам всех видов)
	  collapsed:: true
		- collapsed:: true
		  ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  STATLIBS=./staticLibs
		  SHLIBS=./sharedLibs
		  Static_lib_folder_code=./staticLibsSource
		  Shared_lib_folder_code=./sharedLibsSource
		  SOURCESTATIC=$(Static_lib_folder_code)/source
		  SOURCESHARED=$(Shared_lib_folder_code)/source
		  INCDIRSTATIC=$(Static_lib_folder_code)/header
		  INCDIRSHARED=$(Shared_lib_folder_code)/header
		  STATICLIBGEN_name=static
		  SHAREDLIBGEN_name=shared
		  STLIBGEN=$(STATLIBS)/lib$(STATICLIBGEN_name).a
		  SHLIBGEN=$(SHLIBS)/lib$(SHAREDLIBGEN_name).so
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFLAGS_static=$(GENERALFLAGS) $(foreach D,$(INCDIRSTATIC),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFLAGS_shared=$(GENERALFLAGS) $(foreach D,$(INCDIRSHARED),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  STATICCFILES=$(foreach D, $(SOURCESTATIC), $(wildcard $(D)/*.cpp))
		  SHAREDCFILES=$(foreach D, $(SOURCESHARED), $(wildcard $(D)/*.cpp))
		  OBJECTSSTATIC=$(patsubst %.cpp, %.o, $(STATICCFILES))
		  OBJECTSSHARED=$(patsubst %.cpp, %.o, $(SHAREDCFILES))
		  DEPFILES_libs=$(patsubst %.cpp, %.d, $(STATICCFILES)) $(patsubst %.cpp, %.d, $(SHAREDCFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE)) $(patsubst $(SOURCESTATIC)/%.cpp, $(DEPDIR)/%.d, $(STATICCFILES)) $(patsubst $(SOURCESHARED)/%.cpp, $(DEPDIR)/%.d, $(SHAREDCFILES))
		  LIBSTATIC_files=$(foreach D, $(STATLIBS), $(wildcard $(D)/lib*.a))
		  LIBSHARED_files=$(foreach D, $(SHLIBS), $(wildcard $(D)/lib*.so))
		  LIBSTATIC_names=$(patsubst $(STATLIBS)/lib%.a, %, $(LIBSTATIC_files))
		  LIBSHARED_names=$(patsubst $(SHLIBS)/lib%.so, %, $(LIBSHARED_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(MAKE) libs
		  	$(CPPC) $^ $(foreach D,$(STATLIBS),-L$(D)) $(foreach D,$(SHLIBS),-L$(D)) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) -l$(STATICLIBGEN_name) -l$(SHAREDLIBGEN_name) -o $@
		  	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(SHLIBS)
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final) $(STLIBGEN) $(SHLIBGEN)
		  
		  libs: $(OBJECTSSTATIC) $(OBJECTSSHARED)
		  	mv $(DEPFILES_libs) $(DEPDIR)
		  	ar rc $(STLIBGEN) $(OBJECTSSTATIC)
		  	ranlib $(STLIBGEN)
		  	$(CPPC) -shared -o $(SHLIBGEN) $(OBJECTSSHARED)
		  	rm -rf $(OBJECTSSTATIC) $(OBJECTSSHARED)
		  
		  sh%.o:sh%.cpp
		  	$(CPPC) $(CFLAGS_shared) -fPIC -c $< -o $@
		  
		  st%.o:st%.cpp
		  	$(CPPC) $(CFLAGS_static) -c $< -o $@
		  
		  -include $(DEPFILES_final) 
		  ```
			- file:///home/andrew/MasterFolder/f.txt
	- ## Мастер статист (только чужие статические библиотеки и статические библиотеки, которые пишешь сам)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  STATLIBS=./staticLibs
		  Static_lib_folder_code=./staticLibsSource
		  SOURCESTATIC=$(Static_lib_folder_code)/source
		  INCDIRSTATIC=$(Static_lib_folder_code)/header
		  STATICLIBGEN_name=static
		  STLIBGEN=$(STATLIBS)/lib$(STATICLIBGEN_name).a
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFLAGS_static=$(GENERALFLAGS) $(foreach D,$(INCDIRSTATIC),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  STATICCFILES=$(foreach D, $(SOURCESTATIC), $(wildcard $(D)/*.cpp))
		  OBJECTSSTATIC=$(patsubst %.cpp, %.o, $(STATICCFILES))
		  DEPFILES_libs=$(patsubst %.cpp, %.d, $(STATICCFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE)) $(patsubst $(SOURCESTATIC)/%.cpp, $(DEPDIR)/%.d, $(STATICCFILES))
		  LIBSTATIC_files=$(foreach D, $(STATLIBS), $(wildcard $(D)/lib*.a))
		  LIBSTATIC_names=$(patsubst $(STATLIBS)/lib%.a, %, $(LIBSTATIC_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(MAKE) libs
		  	$(CPPC) $^ $(foreach D,$(STATLIBS),-L$(D)) $(foreach D,$(LIBSTATIC_names),-l$(D)) -l$(STATICLIBGEN_name) -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final) $(STLIBGEN)
		  
		  libs: $(OBJECTSSTATIC) $(OBJECTSSHARED)
		  	mv $(DEPFILES_libs) $(DEPDIR)
		  	ar rc $(STLIBGEN) $(OBJECTSSTATIC)
		  	ranlib $(STLIBGEN)
		  	rm -rf $(OBJECTSSTATIC)
		  
		  st%.o:st%.cpp
		  	$(CPPC) $(CFLAGS_static) -c $< -o $@
		  
		  -include $(DEPFILES_final)
		  ```
	- ## Мастер динамист (только чужие динамические библиотеки и динамические библиотеки, которые написал сам)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  SHLIBS=./sharedLibs
		  Shared_lib_folder_code=./sharedLibsSource
		  SOURCESHARED=$(Shared_lib_folder_code)/source
		  INCDIRSHARED=$(Shared_lib_folder_code)/header
		  SHAREDLIBGEN_name=shared
		  SHLIBGEN=$(SHLIBS)/lib$(SHAREDLIBGEN_name).so
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFLAGS_shared=$(GENERALFLAGS) $(foreach D,$(INCDIRSHARED),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  SHAREDCFILES=$(foreach D, $(SOURCESHARED), $(wildcard $(D)/*.cpp))
		  OBJECTSSHARED=$(patsubst %.cpp, %.o, $(SHAREDCFILES))
		  DEPFILES_libs=$(patsubst %.cpp, %.d, $(SHAREDCFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE)) $(patsubst $(SOURCESHARED)/%.cpp, $(DEPDIR)/%.d, $(SHAREDCFILES))
		  LIBSHARED_files=$(foreach D, $(SHLIBS), $(wildcard $(D)/lib*.so))
		  LIBSHARED_names=$(patsubst $(SHLIBS)/lib%.so, %, $(LIBSHARED_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		      @echo
		  	@echo
		  	@echo
		  	@echo RUN THIS COMMAND:
		  	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(SHLIBS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(MAKE) libs
		  	$(CPPC) $^ $(foreach D,$(SHLIBS),-L$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) -l$(SHAREDLIBGEN_name) -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final) $(SHLIBGEN)
		  
		  libs: $(OBJECTSSTATIC) $(OBJECTSSHARED)
		  	mv $(DEPFILES_libs) $(DEPDIR)
		  	$(CPPC) -shared -o $(SHLIBGEN) $(OBJECTSSHARED)
		  	rm -rf $(OBJECTSSHARED)
		  
		  sh%.o:sh%.cpp
		  	$(CPPC) $(CFLAGS_shared) -fPIC -c $< -o $@
		  
		  -include $(DEPFILES_final)
		  ```
	- ## Альфонсик (только чужие динамические библиотеки и чужие статические библиотеки)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  STATLIBS=./staticLibs
		  SHLIBS=./sharedLibs
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE))
		  LIBSTATIC_files=$(foreach D, $(STATLIBS), $(wildcard $(D)/lib*.a))
		  LIBSHARED_files=$(foreach D, $(SHLIBS), $(wildcard $(D)/lib*.so))
		  LIBSTATIC_names=$(patsubst $(STATLIBS)/lib%.a, %, $(LIBSTATIC_files))
		  LIBSHARED_names=$(patsubst $(SHLIBS)/lib%.so, %, $(LIBSHARED_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		      @echo
		  	@echo
		  	@echo
		  	@echo RUN THIS COMMAND:
		  	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(SHLIBS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(CPPC) $^ $(foreach D,$(STATLIBS),-L$(D)) $(foreach D,$(SHLIBS),-L$(D)) $(foreach D,$(LIBSTATIC_names),-l$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final)
		  
		  -include $(DEPFILES_final)
		  ```
	- ## Альфонсик статист (только чужие статические библиотеки)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  STATLIBS=./staticLibs
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE))
		  LIBSTATIC_files=$(foreach D, $(STATLIBS), $(wildcard $(D)/lib*.a))
		  LIBSTATIC_names=$(patsubst $(STATLIBS)/lib%.a, %, $(LIBSTATIC_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(CPPC) $^ $(foreach D,$(STATLIBS),-L$(D)) $(foreach D,$(LIBSTATIC_names),-l$(D)) -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final)
		  
		  -include $(DEPFILES_final)
		  ```
	- ## Альфонсик динамист (только чужие динамические библиотеки)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  SHLIBS=./sharedLibs
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE))
		  LIBSHARED_files=$(foreach D, $(SHLIBS), $(wildcard $(D)/lib*.so))
		  LIBSHARED_names=$(patsubst $(SHLIBS)/lib%.so, %, $(LIBSHARED_files))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		      @echo
		  	@echo
		  	@echo
		  	@echo RUN THIS COMMAND:
		  	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(SHLIBS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(CPPC) $^ $(foreach D,$(SHLIBS),-L$(D)) $(foreach D,$(LIBSHARED_names),-l$(D)) -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final)
		  
		  -include $(DEPFILES_final)
		  ```
	- ## Какие нахуй библиотеки (никаких библиотек)
	  collapsed:: true
		- ```
		  OUTPUT=prog
		  ROOTDIR=.
		  SOURCEDIR=./lib
		  CODEDIRS=$(ROOTDIR) $(SOURCEDIR)
		  INCDIRS=. ./include/
		  DEPDIR =./deps
		  CPPC=g++
		  C++standart=-std=c++20
		  OPT=-O2
		  DEPFLAGS=-MP -MD
		  GENERALFLAGS=-Wall -Werror -Wextra $(C++standart)
		  CFLAGS=$(GENERALFLAGS) $(foreach D,$(INCDIRS),-I$(D)) $(OPT) $(DEPFLAGS)
		  CFILESROOT=$(foreach D, $(ROOTDIR), $(wildcard $(D)/*.cpp))
		  CFILESSOURCE=$(foreach D, $(SOURCEDIR), $(wildcard $(D)/*.cpp))
		  CFILES=$(CFILESROOT) $(CFILESSOURCE)
		  OBJECTS=$(patsubst %.cpp, %.o, $(CFILES))
		  DEPFILES=$(patsubst %.cpp, %.d, $(CFILES))
		  DEPFILES_final=$(patsubst .%.cpp, $(DEPDIR)%.d, $(CFILESROOT)) $(patsubst ./lib%.cpp, $(DEPDIR)%.d, $(CFILESSOURCE))
		  
		  all: $(OUTPUT)
		  	rm -rf $(OBJECTS)
		  
		  $(OUTPUT): $(OBJECTS)
		  	mv $(DEPFILES) $(DEPDIR)
		  	$(CPPC) $^ -o $@
		  
		  %.o:%.cpp
		  	$(CPPC) $(CFLAGS) -c -o $@ $<
		  
		  mrproper:
		  	rm -rf $(OUTPUT) $(OBJECTS) $(DEPFILES_final)
		  
		  -include $(DEPFILES_final)
		  ```