2021-07-21 16:16
#tag
# Jenkins
### [Install to Linux](https://www.jenkins.io/doc/book/installing/linux/)
- install [[Java]]

### Install Jenkins
```shell
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ >     /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins

sudo service jenkins status
					 start/stop
					 

```
_____________
#### Links
[Установка Jenkins на Ubuntu](https://www.dmosk.ru/miniinstruktions.php?mini=jenkins-ubuntu)
[[Allure]]