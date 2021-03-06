2021-01-09 16:25
[doc_ru](https://routerus.com/gitignore-ignoring-files-in-git/)
# Gitignore

#### Пример .gitignore файла
```shell
.idea/		# игнор папки и файлов в ней
			"игнор файлов в папке" # У gitignore есть такая особенность, что нельзя в исключения добавить папку, если родительская папка игнорируется.
/wp-content/*	# добавляем все файлы внутри папки
!/wp-content/	# затем снимаем игнор с этой папки
			
foo.txt		# Игнорировать файл foo.txt.
*.html		# Игнорировать html файлы
!foo.html	# Но конкретно foo.html не игнорировать
/*.rar		# Игнорировать rar файлы в корне проекта
/bar/*.css	# Игнорировать css файлы из папки bar не включая подпапки
/bar/**.*.js# Игнорировать js файлы из папки bar и подпапок, если таковые будут
```
## Игнорирование ранее зафиксированных файлов
Чтобы проигнорировать файл, который был ранее зафиксирован, вам нужно будет удалить его из индекса, а затем добавить правило для файла в `.gitignore`:

```bash
# Если вы хотите удалить файл как из индексной, так и из локальной файловой системы, опустите опцию `--cached`.
git rm --cached filename 	# --cached говорит не удалять файл из рабочего дерева, а только удалять его из индекса

git rm -r --cached filename # рекурсивное удаления каталога используйте параметр `-r`:
git rm -r -n directory		# `-n`, которая выполнит пробный запуск и покажет, какие файлы будут удалены:
```
.gitignore нужен для скрытия файлов и папок от системы контроля версий [Git](http://tyapk.ru/blog/post/learning-git). Обычно скрывают конфигурационные файлы (особенно с паролями), временные файли и папки. gitignore использует [glob формат](https://www.wikiwand.com/en/Glob_(programming)) для выборки файлов.

[ Игнорирование файлов и каталогов в Git](https://andreyex.ru/linux/ignorirovanie-fajlov-i-katalogov-v-git-gitignore/)

[gitignore Manual Page](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html)

#### Link
[[Git]]
https://devacademy.ru/article/ignorirovanie-faylov-i-katalogov-v-git 
