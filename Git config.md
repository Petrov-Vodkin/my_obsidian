2021-06-03 17:09
#tag
# git config
```powershell
								"Dir of cfg"
cat ~/.gitconfig	# global scoop
~/.config/hub		# токен/пароль & имя пользователя
```
```git config
[alias]
	a = add
	br = branch
	co = checkout
	cm = commit
	g = log --graph --abbrev-commit --decorate --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(dim white) - %an%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n %C(white)%s%C(reset)'
	ph = push
	s = status --short
	st = status
	l = log --oneline --graph --decorate --all

```
```bash
									"bash"
gitpush() {
    git add .
    git commit -m "$*"
    git push
}
alias gp=gitpush
```
```powershell
									"powershell"
function gm() {
     git commit -am "$1" && git push
}
# gm "This is my message"
```
_____________
#### Links