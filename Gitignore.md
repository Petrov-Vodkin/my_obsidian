2021-01-09 16:25
#gitignore 
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

.gitignore нужен для скрытия файлов и папок от системы контроля версий [Git](http://tyapk.ru/blog/post/learning-git). Обычно скрывают конфигурационные файлы (особенно с паролями), временные файли и папки. gitignore использует [glob формат](https://www.wikiwand.com/en/Glob_(programming)) для выборки файлов.

[ Игнорирование файлов и каталогов в Git](https://andreyex.ru/linux/ignorirovanie-fajlov-i-katalogov-v-git-gitignore/)

[gitignore Manual Page](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html)

#### [[Git]]
