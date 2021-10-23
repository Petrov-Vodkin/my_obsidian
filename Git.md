2020-12-07 15:37
#git #гит #github
# GIT
### [[Откатиться GIT]] 
### [[Git config]]
### [[Start_git]]

```bash
git add "name || dir" 	# забросить файл name или dir  
git add . 				# все требующие коммит
git commit -m "message" # закомитить
	git commit -am 		# добавит и зафиксирует все изменить в репозиторий
git push #  отправит добавленные фвйлы в репо
git pull # получить файлы  
git rm -r --cached 'name' #  удалить 'name' (папку\файлы) из репо 
git commit --amend -m 'New commit message' #  перезапишет сообщение последнего коммита
								"Пример"
			git commit -m 'Initial commit'
			git add forgotten_file
			git commit --amend
								"ОТКАТ"
# до команды add .
git checkout .			# откатиться()(. == все файлы || указать конкретный фаил для отката изменений)к текущему коммиту
# после add .
git reset file_name		# вытащить фаил из стэйджа
git checkout file_name	# откатиться 
# после commit
git reset --hard HEAD^1	# жёстко вернуться на 1н(HEAD^1) commit назад
		  --soft HEAD^1	# изменения в файле остаются
		  						"ВЕТКИ"
git branch					# вывести: все ветки
			-v				# вывести: ветка + коммит
git branch name_banch		# создать ветку name_banch
git checkout name_banch		# подключиться к name_banch
git checkout -b name_branch	# создать ветку name_banch & подключиться к ней
git branch -m NEW_NAME		# переименовать активную ветку
git checkout HashCommit		# откатиться к коммиту

git push 					# сохранить изменения(добавить) активной ветки в репо

git branch -D name_branch	# Dell branch - name_branch
```
`gitk` — графическая утилита, которая показывает наш граф. В качестве ключей передаём имена веток или `--all`, чтобы показать все.
 ```console
git init
git commit -m 'Initial project version'
```
 ### Добавление репозитория на GitHub[](https://askdev.ru/q/mozhno-li-sozdat-udalennoe-repo-na-github-iz-cli-bez-otkrytiya-brauzera-4384/)
```shell
git config --list # all inf (name, mail ......)
git init	# инициализировать репозиторий в dir(проэкта)
# нужен токен(а не пароль) получаем в своём репо(scope==repo)
https://github.com/settings/tokens 			# получить токен
https://github.com/settings/applications	# отозвать
~/.config/hub 			# токен/пароль & имя пользователя
# создаём repo 
curl -u 'Petrov-Vodkin' https://api.github.com/UserName/repos -d'{"name":"RepoName"}' # - d это параметр curl, который позволяет отправлять почтовые данные с запросом
# или через hub
hub create 	# hub itstall in scoope | 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 
# добавить определение местоположения созданного выше репозитория
git remote add origin https://github.com/UserName/RepoName 
git push origin master
# Переходим в проект
git add .	# Добавляем файлы в репозиторий (все файлы)
git commit -m "Initialise project version"` # Делаем первый коммит
git status
```  
 ### [Как правильно составлять описания коммитов и почему это важно](https://ru.hexlet.io/blog/posts/git-commit-message)
 1. оставляйте пустую строку между заголовком и описанием
 2. ограничивайте длину заголовка 50 символами
 3. пишите заголовок с прописной (заглавной) буквы
 4. не ставьте точку в конце заголовка описания
 5. используйте повелительное наклонение в заголовке(Сделай, Закрой, Исправь)
 6. ограничивайте длину строки в теле описания 72 символами
 7. в теле описания отвечайте на вопросы «что?» и «почему?», а не «как?»
 [альтернативный источник](https://medium.com/grisme/%D0%BA%D0%B0%D0%BA-%D0%BF%D0%B8%D1%81%D0%B0%D1%82%D1%8C-%D1%81%D0%BE%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D0%BE%D0%B2-%D0%B2-git-9ed19ebc5ebf)
### [**hub**](https://hub.github.com/)
— консольное приложение, упрощающее введение команд git и позволяющее производить некоторые недоступные для git действия в удалённых репозиториях из терминала
#### Links
[[Gitignore]]
[Video Уроки по GIT](https://www.youtube.com/playlist?list=PLuY6eeDuleIOMB2R_Kky05ZfiAx2_pbAH)
[Git для начинающих](https://webdevkin.ru/courses/git/git-commit)
[Exempl commit](https://github.com/tpope/vim-pathogen/commits/master)
[терминал - создание repo](https://askdev.ru/q/mozhno-li-sozdat-udalennoe-repo-na-github-iz-cli-bez-otkrytiya-brauzera-4384/)
[Habr](https://habr.com/ru/post/60347/)-_-----_-Книга[](https://git-scm.com/book/ru/v2)-_-----_-[Базовые операции:](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/ru/ch03.html)