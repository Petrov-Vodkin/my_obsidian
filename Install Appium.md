2021-06-17 19:28
#tag
# Install Appium
1[[Android Studio]] + настроить sdk [[android-studio-install-14-sdk-tools-768x521.png]]
```
ANDROID_HOME    # имя
%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools # переменная
```
2[[JDK]] - [download](https://www.oracle.com/java/technologies/javase-downloads.html). После установки **_нужно установить переменные окружения_** - [как прописать JAVA_HOME](http://kesh.kz/blog/how-to-set-java-home/).
```powershell
   						'в системные переменные среды'
JAVA_HOME # имя 
C:\Program Files\Java\jdk-16.0.1 # переменная
						'добавь в саму перменную PATH'
%JAVA_HOME%\bin
C:\Users\Di13\AppData\Local\Android\Sdk\tools
C:\Users\Di13\AppData\Local\Android\Sdk\tools\bin # для sdkmanager
C:\Users\Di13\AppData\Local\Android\Sdk\platform-tools
C:\Users\Di13\AppData\Local\Android\Sdk\build-tools
# проверь в терм run:  	adb
#						java --version
```
2.[Download appium-desktop](https://github.com/appium/appium-desktop/releases/tag/v1.21.0)
   [Appium Desired Capabilities](https://appium.io/docs/en/writing-running-appium/caps/)
3.Use [[Appium-doctor]] 
4.Device Settings
create virt dev without [[AVD Emulator]]  || [[VirtualOS]]

### Требования для тестирования Android на Windows и Mac
-   Java (версии 7 и выше)
-   Android SDK API (версии 17 и выше)
-   Android Virtual Device (AVD) \[эмулятор, как минимум, доступен после установки Android Studio или реальное устройство
### Установка Appium для работы с Android
-   JDK (Java development kit)
-   Android SDK (Software development kit)
-   Appium под разные операционные системы
_____________
#### Links
[[Appium]]