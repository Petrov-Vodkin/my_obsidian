2021-06-03 14:30
#tag
# Откатиться GIT
#### Clear Git History
```bash 
# Create a temporary branch and `checkout`:
git checkout --orphan temp_branch
# Add all files to the temporary branch and commit the changes:
git add -A
git commit -am "The first commit"
# Delete the `main` branch:
git branch -D main
# Rename the temporary branch to `main`:
git branch -m main
# Forcefully update the remote repository:
git push -f origin main
```
#### V2
```bash
				"без add = без добавления в stage" # local 
git checkout -- file_name 	# откат выбранрого файла в состояние последнего commit'a (сохранённого в репо - если обновлял) (сохранённого локально)
git checkout . 				# откат всех файлов 
						"после add" # local
git reset filename		# filename удаляется из stage (отменяется команда add filename)
git checkout filename	# откат файла в состояние последнего commit'a
						"после commit" # local
git reflog # выдаст список HEAD c номерами и описанием, достаточно выбрать интересуещее вас состояние и сделать сброс до этого HEAD.
#1
git reset --hard HEAD^1	# на 1 коммит назад + откат файлов 
git reset --soft HEAD^1	# на 1 коммит назад, файлы не изменяются
#после можно отдельно удалить файлы из stage и откатить изменения в них ::::::: git reset .     &&       git checkout -- . 
#2
git reset --hard HEAD @{`номер`}
					"после push" # откат репозитория !!!!
#№1
git push --force # Перед этим нужен `git reset`
#№2
git revert номер_проблемного_коммита # Создаёт второй, "противоположный" коммит, "со знаком минус". После его публикации получится состояние, как до проблемного коммита, но в истории останется пара ненужный-коммит + отмена-ненужного-коммита.
```
```bash
				"Если не обновляет репо после сброса"
git add .
git commit -am 'message'
git push -u origin main
```
_____________
#### Links
[[Git]]
