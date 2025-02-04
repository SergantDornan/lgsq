# Что это
	- Maven - система сборки проектов на java.
	- [установка](https://maven.apache.org/install.html)
	- Вся полезная информация про сборку хранится в конфигурационном файле pom.xml
- # Как создать пустой проект, собираемый maven
	- ```
	  mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5 -DinteractiveMode=false
	  ```
	- Здесь редактируешь две опции:
	  ``-D groupId`` - это условно название корпорации, которая делает проект. Если написать com.mycompany.app в src будет дохуя вложенных папок com/mycompany/app. Как я понял, это используется и для разделения разных крупных проектов одной комании, типа у org.springframework есть org.springframework.boot и spring.framework.data
	- ``-D artifactId`` - это название конкретного отдельного продукта (типа библиотека, приложение, пакет и т.д.)
- # Как скомпилировать
	- Пишешь это скомпилировать:
	- ```
	  mvn compile
	  ```
	- Либо это, чтобы сначала он скачал все зависимости, написанные в pom.xml, скомпилировал и запустил тесты
	- ```
	  mvn install
	  ```
- # Как запустить
	- ### Руками джавой
		- Можно просто запустить с помощью java (именно это сделает IDE когда ты нажмёшь запуск)
		- ```
		  java --class-path ./target/classes/ [имя класса, напрмер com.mycompany.app.Main]
		  ```
	- ### Добавить mainClass
		- Можно добавить строчку ``<exec.mainClass>com.mycompany.app.Main</exec.mainClass>`` в ``<properties>``:
		- ```
		    ...
		    <properties>
		      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		      <maven.compiler.release>17</maven.compiler.release>
		      
		      <exec.mainClass>com.mycompany.app.Main/exec.mainClass>
		    </properties>
		    ...
		  ```
	- ### Через опцию
		- Можно передать её через опцию командной строки:
		- ```
		  mvn exec:java -Dexec.mainClass="com.mycompany.app.Main"
		  ```
	- ### Через плагин
		- Можно в ``<plugins>`` добавить плагин exec-maven-plugin:
		- ```
		  <plugin>
		    <groupId>org.codehaus.mojo</groupId>
		    <artifactId>exec-maven-plugin</artifactId>
		    <version>1.4.0</version>
		    <configuration>
		      <mainClass>org.dhappy.test.NeoTraverse</mainClass>
		    </configuration>
		  </plugin>
		  
		  ```
		- А потом запустить ``mvn exec:java``
	-
- # Как прикрутить зависимость
	- Допустим я хочу прикрутить к проекту springframework.boot
	- Надо зайти на сайт https://mvnrepository.com/
	- Там найти нужный пакек, выбрать версию, которая тебе нужна, и там будет конфигурация, которую нужно скопипастить в ``<dependencies>``
	- ```
	  <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter -->
	  <dependency>
	  	<groupId>org.springframework.boot</groupId>
	  	<artifactId>spring-boot-starter</artifactId>
	  	<version>3.4.2</version>
	  </dependency>
	  ```
	-
- # Переменные конфигурации
	- Задаются в ``<properties>``, вот таким образом:
	- ```
	  <properties>
	  	...
	      <variable-name>variable-value</variable-name>
	      ...
	  </properties>
	  ```
	- Подставляются вот таким образом (как в bash):
	- ```
	  $(variable-name)
	  ```
-