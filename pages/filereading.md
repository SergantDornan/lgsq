- ```c++
  #include <fstream>
  ```
- # Запись
	- ```c++
	  int main()
	  {
	      std::ofs eam out;          // поток для записи
	      out.open("hello.txt");      // открываем файл для записи
	      if (out.is_open())
	      {
	          out << "Hello World!" << std::endl;
	      }
	      out.close(); 
	      std::cout << "File has been written" << std::endl;
	  }
	  ```
- # Чтение из файла
	- ```c++
	  int main()
	  {
	      std::string line;
	   
	      std::ifstream in("hello.txt"); // окрываем файл для чтения
	      if (in.is_open())
	      {
	          while (std::getline(in, line))
	          {
	              std::cout << line << std::endl;
	          }
	      }
	      in.close();     // закрываем файл
	  }
	  ```