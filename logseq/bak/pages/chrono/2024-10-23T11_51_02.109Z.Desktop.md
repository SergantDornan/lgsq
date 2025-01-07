# Duration
collapsed:: true
	- Можно создать переменную, в которой будет хранится некоторое количество времени
	  
	  ```c++
	  std::chrono::seconds five_seconds = srd::chrono::seconds(5);
	  ```
	  Можно не только секунды, но и миллисекунды, микросекунды, наносекунды
	- Для сокращения можно писать 
	  ```c++
	  std::chrono::seconds five_seconds = 5s;
	  ```
	  Или
	  ```c++
	  auto five_seconds = 5s;
	  ```
	  Буквы есть не только для секунд, но и для наносекунд, микросекунд и тд
	- Промежутки времени можно переводить в дргие промежутки
	  ```c++
	  auto day = 24h;
	  auto sec = std::chrono::seconds(day); // перевели 24 часа в секунды
	  ```
	  Можно переводить только менее точный тип времени в более точный (то есть например можно часы в секунды, но нельзя секунды в часы)
	  
	  Но все таки если очень хочется можно сделать так
	  ```c++
	  day = duration_cast<hours>(sec);
	  ```
- # Time points
	- Установка настоящей временной точки
	  ```c++
	  auto time_point = std::chrono::system_clock::now();
	  auto time_p1 = std::chrono::steady_clock::now();
	  
	  ```
	- Cущесвтует несколько видов часов, откуда можно брать время. System clock, steady clock, high resolution clock, использовать системные для больших промежутков времени, остальные - для точных вычислений
	- Можно узнать сколько времени прошло с начала эпохи (какая то начальная точка во времени внутри компа)
	  ```c++
	  auto time_point = std::chrono::system_clock::now(); // Установка временной точки
	  std::cout << duration_cast<std::chrono::days>(time_point.time_since_epoch()).count() << std::endl;
	  // вывели на экран сколько прошло времени
	  ```
- # Промежутки времени
	- ```c++
	  auto old = std::chrono::steady_clock::now();
	  // some code
	  auto dur = std::chrono::steady_clock::now() - old;
	  std::cout << dur.count() << std::endl;
	  ```
	  
	  Ответ должен получиться в наносекундах
	- Когда вычитаешь time_point из другой time_point ты получаешь duration, а не time_point