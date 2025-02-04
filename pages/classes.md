# Модификаторы доступа полей и методов
collapsed:: true
	- ### private
		- Никому ниче не доступно
		- ```
		  class Penis {
		  	private void vasectomy() {}
		  }
		  ```
	- ### public
		- Ясен красен, всё доступно всем
		- ```
		  class Penis {
		  	public void kiss_the_penis();
		  }
		  ```
	- ### protected
		- Доступно если ты: (наследник) ИЛИ (в том же пакете)
		- ```
		  package pack1;
		  
		  class Penis {
		  	protected void tickle_balls() {}
		  }
		  ```
		- Тот же пакет:
		- ```
		  package pack1;
		  
		  class Bro {
		  	publuc void gay_behaviour() {
		      	Penis penis = new Penis();
		        	penis.tickle_balls();		// OK
		      }
		  }
		  ```
		- Наследник:
		- ```
		  package pack2;
		  import pack1.Penis;
		  
		  class BigBlackCock extends Penis {
		  	public void morning_routine() {
		      	tickle_balls();				// OK
		      }
		  }
		  ```
		- Не наследник и другой пакет
		- ```
		  package pack3;
		  import pack1.Penis;
		  
		  class GothGirl {
		  	public void try_being_nice() {
		      	Penis penis = new Penis;
		          penis.tickle_balls();		// error
		      }
		  }
		  ```
-
- # Модификаторы доступа классов
  collapsed:: true
	- Обычный класс без наследников может быть только public:
	- ```
	  public class Hookah; // same thing as "class Hookah"
	  ```
	- private и protected может быть вложенный класс
	  ```
	  public class Hookah
	  {
	  	private class Flavour;
	  }
	  ```
	-
-