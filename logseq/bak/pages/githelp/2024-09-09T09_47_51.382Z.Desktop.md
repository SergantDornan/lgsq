## Установка локального репозитория и соеденение с удаленным
collapsed:: true
	- git init
	  git config --global user.name "SergantDornan"
	  git config --global user.email "microsoftvis@gmail.com"
	  git remote add origin <repository url>
	  
	  Затем надо ввести команду из след файла (гит не разрешает пушить если тут секретный код):
	  file:///home/andrew/MasterFolder/secretcode
- ## Начальный push / clone
  collapsed:: true
	- git remote add origin <url>
	  git branch -M main
	  git push -u origin main
	  git push -f -u origin <name of branch>     (force)
	  git add --all
	  git commit -m "хуйня"
	  
	  !!!uncharted:
	  или:
	     git clone <URL-адрес-репозитория>
	  или:
	  git clone -v <url>
- ## Откат к пред версиям
  collapsed:: true
	- git reset --hard HEAD~1  (откат к предыдущему коммиту с удалением коммита)
	  git reset HEAD~1 (откат к пред коммиту без удаления из истории)
	  git log --oneline
	  git reset --soft commitID - мягкий откат
	  git reset --hard commitId - жесткий откат
	  git reflog - история действий
	  git reset commitID --hard - удалить коммит
- ## Ветки
  collapsed:: true
	- git checkout -b <имя_ветки>
	  **EXPL**
	  git checkout переключает вас на существующую ветку или создает новую.
	  b указывает, что нужно создать новую ветку.
	  <имя_ветки> - название ветки, которую вы хотите создать.
	  **ENDEXPL**
	  
	  git checkout <имя_ветки> - переключиться на другую ветку
	  git branch -d <имя_ветки> - удалить ветку
	  git merge <имя_ветки>
	  **EXPL**
	  git merge используется для слияния двух веток.
	  <имя_ветки> - название ветки, которую вы хотите слить с текущей веткой.
	  **ENDEXPL**
- # Gitignore
  collapsed:: true
	- git add .gitignore
	  
	  Символ `#`: указывает на комментарий.
	  Пустые строки: игнорируются.
	  Шаблоны: используются для соответствия файлам и директориям.
	  Звездочка `*`: соответствует любому количеству любых символов.
	  Символ вопроса `?`: соответствует любому одному символу.
	  Квадратные скобки `[]`: соответствуют диапазону символов (например, `[a-z]` - любые строчные буквы).
	  Косая черта `/`: обозначает корень директории.
	  Двойная звездочка `**`: соответствует любой вложенности директорий.
	  
	  Примеры:
	  ```
	  Игнорировать все файлы .log
	  *.log
	  Игнорировать все файлы в папке tmp
	  tmp/
	  Игнорировать файл my_secret.txt
	  my_secret.txt
	  Игнорировать все файлы, начинающиеся с .
	  .*
	  ```
- # SSH
  collapsed:: true
	- ## SSH setup
		- ssh-keygen -t ed25519 -C "email"  - generate ssh key
		  cat <SSH_file_name.pub>  - show SSH key (exmp cat ~/.ssh/id_ed25519.pub)
		  
		  Нам нужно скопировать все содержимое файла .pub
		  Дальше GitHub -> profile picture -> settings -> SSH and GPG keys -> new SSH key
	- Дальше можно делать что угодно пока ssh хранится на компе в виде файла и он есть в гитхабе, клонировать репозитории через ssh, пушить и тд, он сам разберется
	- git clone SSH (if it is someones repository)