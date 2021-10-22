2021-08-05 15:13
#tag
# Java
### [](https://www.jenkins.io/doc/book/installing/linux/#installation-of-java)Installation of Java[](https://www.jenkins.io/doc/book/installing/linux/#installation-of-java)

Jenkins requires Java in order to run, yet certain distributions don’t include this by default and [some Java versions are incompatible](https://www.jenkins.io/doc/administration/requirements/java/) with Jenkins.

There are multiple Java implementations which you can use. [OpenJDK](https://openjdk.java.net/) is the most popular one at the moment, we will use it in this guide.

To install the Open Java Development Kit (OpenJDK) run the following:
```shell
#   Update the repositories
sudo apt update
#   search of all available packages:
sudo apt search openjdk
#   Pick one option and install it:
sudo apt install openjdk-11-jdk
#   Confirm installation:
sudo apt install openjdk-11-jdk
#   checking installation:
java -version
```
### Start Jenkins[](https://www.jenkins.io/doc/book/installing/linux/#start-jenkins)
```shell
# Register the Jenkins service with the command:
sudo systemctl daemon-reload
# You can start the Jenkins service with the command:
sudo systemctl start jenkins
# You can check the status of the Jenkins service using the command:
sudo systemctl status jenkins
# If everything has been set up correctly, you should see an output like this:
Loaded: loaded (/etc/rc.d/init.d/jenkins; generated)
Active: active (running) since Tue 2018-11-13 16:19:01 +03; 4min 57s ago
```
_____________
#### Links
[[]]