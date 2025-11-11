1. Get started
2. first step
3. commits
4. Gitignore
5. branches


# Get started

	download Git, open console,  write - `git --version`

# First step

напишите `git --help` для того что бы увидеть весь список команд

для того что бы воспользоваться гитом в нашем проекте нужно его инициализировать  - `git init`.
после чего гит инициализируется в корне папки проекта, где и будут храниться все изменения.

команда `git status` позволяет нам узнать важную информацию, а именно:
1. На какой ветке мы находимся
	
2. Какие коммиты были совершенны
	
3. Информация о файлах в проекте

пример
```git
name@name-device the-file-you-in % git status 
On branch master  <--- информация о ветке, где - `On branch "имя ветки"`

No commits yet   <--- информация о коммитах 

Untracked files:   <--- информация о файлах
	(use "git add <file>... " to include in what will be commited)
		untracked_file.html		
```


для того что бы начать "следить" за файлом нужно его добавить при помощи команды - `git add <file>... `

```git
name@name-device the-file-you-in %  git add test.html
name@name-device the-file-you-in % git status 

On branch master  

No commits yet

Changes to be committed:
	(use "rm --cached <file>..." to unstrage)
		new file: test.html
```

>[!note] Обратите внимание
>При любой модификации файла, для дальнейшей работы с гитом - файл требуется снова "фиксировать"  той же командой `git add` или `git restore`.
>`git restore` - просто заменит файл последней зафиксированной версией


для того что бы перестать "следить" за файлом нужно его добавить при помощи команды - `git add <file>... `

```git
name@name-device the-file-you-in %  rm --cached test.html
name@name-device the-file-you-in % git status 

On branch master  

No commits yet

Untracked files:   <--- информация о файлах
	(use "git add <file>... " to include in what will be commited)
		test.html		
```


# Commit

для того что бы сохранить файл в гите - `git commit`
команда может принимать разные параметры:
1.  -m

```git
name@name-device the-file-you-in % git commit -m "first commit"
[master (root-commit) hash-code] first coomit
2 files where changed, 19 insertions
create mode
create mode
```

# Gitingnore

для того что бы игнорировать ненужные нам для гита файлы нужно:
1. создать файл - `.gitignore`
2. добавить его в репозиторий `git add .gitignore`
3. в этом файле нужно записать папки, файлы которые нам не интересны
	- для файлов нужно написать название файла и его расширение
	- для папок нужно добавить путь `/logs`


# Branches

для того что бы увидеть на какой мы ветке `git branch`

для того что бы создать ветку `git branch название`

для того что бы удалить ветку `git branch -D название`

для того что бы перейти на ветку `git checkout название`

Что бы добавить файл в ветку которая нам нужна нужно находиться в этой ветке после чего добавить файл


# Merge

`git merge название` - для того что бы совместить ветки.
Все файлы теперь будут доступны другой ветке


# Github

git - это локальное приложение
github - это как соцсеть для разработчиков

для того что бы синхронизировать git с github-ом :
1. создать в githab-е репозиторий 
2. зарегистрироваться через git 
	 `git config --global user.name "Your user_name" user.emai "Your email"`
	 нужно ввести те же данные что и в github
3. написать `git remote add origin https://your_repository_url` для удаленного доступа
4. для того что бы локально залить наши изменения на сервер `git push -u origin master`
5. git push - для сохранения изменений на сервере git pull - для того что бы забрать измененные данные с сервера
