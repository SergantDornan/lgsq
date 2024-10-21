## incompatible types in assignment of ‘int (*)[3]’ to ‘int [3][3]’
	- ### Код
		- ```
		  class GameState {
		  	int board[3][3];
		  public:
		  	GameState(int arr[3][3]) : board(arr) {}
		  };
		  ```
	- ### Проблема
		- При передаче массива в качестве параметра функции, он всегда приводится к указателю. Копия массива никогда не передаётся в качестве аргумента.
		  https://c-faq.com/aryptr/aryptrparam.html
	- ### Решение
		- ```
		  class GameState {
		  	int board[3][3];
		  public:
		  	GameState(int arr[3][3]) {
		      	std::memcpy(board, arr, 3*sizeof(*arr));
		      }
		  };
		  ```
	-
	-