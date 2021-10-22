2021-02-12 15:42
#скачать
# [wget](http://www.gnu.org/software/wget/manual/wget.html#Types-of-Files)
```bash
wget опции аддресс_ссылки
	 -i ФАЙЛ # Загрузка всех URL, указанных в локальном или внешнем ФАЙЛЕ
	 -b (--background)	#  работать в фоновом режиме
	 -P dir link		# Скачивание файлов в указанный каталог
	 -O /path/for/save/name_file ftp://ftp.example.org/some_file.iso # Скачивание файлов в указанный каталог(save) сохранение под указанным именем(name_file)
	 -b ftp://ftp.example.org/some_file.iso # Скачивание в фоновом режиме 
	 -c ftp://ftp.example.org/some_file.iso # Продолжит скачивание  
	 --spider ftp://ftp.example.org/some_file.iso # провериить доступность файла
	 -r -l num -A gif,jpg
						 # -r рекурсивное скачивание сайта;
						 # -l ЧИСЛО глубина рекурсии (0 - бесконечность);
						 # -A скачать ТОЛЬКО файлы этого формата/ов(gif,jpg); 
						 # -nd без создания директорий(всё в кучу)
wget -A 'zelazny * 196 [0-9] *' # будет загружать только файлы, начинающиеся с «zelazny» и содержащие числа с 1960 по 1969 в любом месте.						 
```
_____________
#### Links
[[Основные консольные команды Linux (Debian)]]
[losst](https://losst.ru/komanda-wget-linux)