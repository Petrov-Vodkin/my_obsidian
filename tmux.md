2021-02-16 13:54
#terminal #tmux #терминал
[link](https://losst.ru/shpargalka-po-tmux)
# Tmux
```bash
tmux 						# создание сессии
tmux new-session -s name	# создание сессии под именем "name"
Ctrl+B 		# режим команд

		c	# создать новое окно
		num	# переключиться на окно под номером num
		w 	# выбрать окно из списка
		,	# переименовать текущее окно
		;	# переключаться между текущей и предыдущей панелью;
	Shift "	# разделить окно гризонтально на 2 панели 
	Shift %	# разделить окно вертикально на 2 панели
	Ctrl => # изменение размера панели(=> это стрелка)
 стрелка(=>)# преключение между панелями
 		[ 	# листать окно (для перехода в режим копирования) 
 
		N	# следующее окно
		P	# предыдущее окно
		D 	# свернуть tmux в фон (без завершения запущенных процессов)
		
		:	# открыть командную строку

tmux ls					# список сессий 
tmux a -t name_session	# вернуться в tmux

exit || X	# закрыть вкладку
```
### Копирование в буфер
```bash
Ctrl+b [ 	# для перехода в режим копирования 
	Ctrl+пробел # для начала выделения
	Ctrl+w 		# копирование
Ctrl+b ]	# вставка
q или Esc	# выход из режима 
```
### Поддержка мышки
Для этого откройте файл `~/.tmux.conf` и добавьте туда следующие строки:
```bash
set-option -g -q mouse on  
bind-key -T root WheelUpPane if-shell -F -t = "#{alternate_on}" "send-keys -M" "select-pane -t =; copy-mode -e; send-keys -M"  
bind-key -T root WheelDownPane if-shell -F -t = "#{alternate_on}" "send-keys -M" "select-pane -t =; send-keys -M"
```
_____________
#### Links
[[Основные консольные команды Linux (Debian)]] 
[[Linux]]
[[Debian]] 