**Appium Tutorial** is ready for all testers! I will use **Appium Desktop** and **Android Studio** to create a sample **mobile test automation** project for you. After that, we will continue with the **Advance Appium Tutorial** series. First,  we need to do a proper **Appium Installation**. Alright, Let’s start!

## **Appium Tutorial Prerequisites: JAVA and Maven Installation**

**Download JAVA JDK first. Here is the Link**: [JAVA JDK 11](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)

Installation steps and configuration settings have described in **[this article](http://www.swtestacademy.com/quick-start-to-selenium-webdriver-with-java-junit-maven-intellij/)** at **step-4 and step-5.** 

> **For MACOS users please visit the below article for all installation needs.** 
> 
> **[https://www.swtestacademy.com/how-to-install-appium-on-mac/](https://www.swtestacademy.com/how-to-install-appium-on-mac/)**

Windows users will go on with the below installation steps. ;)

## **Android Studio Installation** 


After clicking the “Finish” button. Go to **“Configure” > “SDK Manager”** to get SDK information. It is required for Android SDK path settings.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/03/img_5c867de24bf51-600x440.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/03/img_5c867de24bf51.png)

Select your device’s or emulator’s Android API level (Version). We will go with **Android 11 API**, please install that one.

> When took the below screenshot the latest version was 9.0 but for this example, we will go with Android 11. I am not using windows anymore thus I could not update the screenshot but I wanted to mention it to guide you correctly.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/03/img_5c867eb0055d6-600x423.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/03/img_5c867eb0055d6.png)

And select the required tools as shown below and click “OK.

[![appium inspector](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-14-sdk-tools.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-14-sdk-tools.png)

Click OK one more time, please. ;)

[![ios testing](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-15.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-15.png)

Click the Finish button and continue.

[![ios automation](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-16.png "shadow-black")](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-studio-install-16.png)

After the installation of the required tools, go to the SDK Manager page and copy the SDK path as shown below.

[![android automation](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-sdk-location.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-sdk-location.png)

[Download RapidEE tool](https://www.rapidee.com/en/download) and install it and open it as administrator. 

[![android testing](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/rapid-ee.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/rapid-ee.png)

And then add **ANDROID\_HOME** variable and its path should be Android SDK’s path. Also, check your **JAVA\_HOME** variable. **JAVA\_HOME** should equal to JAVA SDK’s path.

[![uiautomator](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-java-home.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-java-home.png)

Then, you need to add **required Android tools** and **JAVA JRE** paths to your **system path** as shown below.

[![selendroid](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-java-path-variables.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-java-path-variables.png)

After that, check your settings and installations. Open a command prompt window and type **“sdkmanager –list”** command as shown below.

[![mobile automation](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/sdkmanager-check.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/sdkmanager-check.png)

and type **“uiautomatorviewer”** to check uiautomatorviewer is working properly.

[![mobile testing tools](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/ui-automatorviewer-check.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/ui-automatorviewer-check.png)

Then, create a sample project in Android Studio and then click the link as shown below to install missing libraries.

[![what is appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-missing-libraries.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/android-missing-libraries.png)

After installation, click the Finish button.

[![download appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-missing-libraries-downloaded.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/andorid-missing-libraries-downloaded.png)

After installing missing libraries you will see the device and little and sweet android icon. :) When you click this icon, you will open the android virtual device manager.

[![install appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/device-emulator-icon-appeared.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/device-emulator-icon-appeared.png)

Let’s create a virtual device. I will also explain how to do mobile automation with a real device too. Don’t worry. ;) Click to “**\+ Create a Virtual Device**” button.

[![apium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/create-a-virtual-device.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/create-a-virtual-device.png)

Then, select a virtual device in the device list.

> **To run ARM-based apk files on X86 platforms (windows or mac),**  
> **please refer to this article: [https://www.swtestacademy.com/how-to-run-arm-apk-on-x86-systems/](https://www.swtestacademy.com/how-to-run-arm-apk-on-x86-systems/)**

After coming back to the above article and after installing your device, you will see the below result.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-7.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-7.png)

Until now, we installed JAVA and Android-related libraries and did their settings and configurations. Now, it is time to download Appium.

## **A****ppium Desktop Installation and Configurations**

Go to [http://appium.io/downloads.html](http://appium.io/downloads.html) and click the “**Appium-Desktop for OSX, Windows, and Linux**” link.

[![download appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/download-appium-1.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/download-appium-1.png)

On the below page, click “**appium-desktop-Setup-1.20.2.exe**” file. (_I am writing this article the latest release is 1.20.2, you can install the latest version when you are installing appium._)

[![appium-desktop-versions](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-3.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-3.png)

When the installation file downloaded, click run and start to install **appium desktop**.

[![install appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-install-1.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-install-1.png)

When installation finished, double-click the appium icon and open the appium server as shown below.

[![appium-desktop-welcome-screen](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-1.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-1.png)

Let’s click the “**Advanced**” tab and change the Server Address to “**127.0.0.1**” and click Allow Session Override for override session when there will be problems and click “Start Server”. If you will use a real device and then use “**0.0.0.0**” for “**Server Adress”**.

[![appium-desktop-settings](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-2.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-2.png)

Set Android and JAVA home in Appium Desktop.

[![android-home-settings](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-4.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-4.png)

Give the required permission to **Appium Server.**

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-install-3.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-install-3.png)

You will see the server up and running.

[![appium-desktop-working](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation.png)

## **Android Virtual Device and Pre-Test Settings**

Before starting the tests you can directly install your apk or xapk files if you have. In our example, first, we will install [apk pure app](https://m.apkpure.com/apkpure/com.apkpure.aegon/download?from=details).

After please open the apk pure app and search for any app you want to install. In this example, I will go with the “Isin Olsun” application. My previous company’s app. :-)

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-5.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-5.png)

After clicking the install button, the app will be installed.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-6.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-6.png)

After these steps, when you click the app icon, you will see that it is opening as expected. :)

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-8.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-8.png)

In order to find the splash activity, I installed the **APK Info** app via APK Pure application. For this, just open the APK pure app, search the “Apk Info” and install the app.

Then, I opened the APK Info app and find our application “**İşin Olsun**” and click it.

Then I saw all the information about our app. When I looked at the Activities section, I saw that our application is starting with “**com.isinolsun.app.activities.SplashActivity**“. I will use this info in the automation code’s desired capabilities section. Also, I will use app package info which is “**com.isinolsun.com**“

[![android-activities](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-9.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-9.png)

Then, open a command prompt and write “**adb device**s” command to see connected devices and get the device ID as shown below.

[![mobile devices in automation](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/device-id.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/device-id.png)

Go to your AVD’s setting tab and check your AVD’s Android Version as shown below. We will use these settings in our test project.

[![android-version](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-10.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-10.png)

Then, open IntelliJ IDE and create a new project as shown below. First, select Maven and click Next.

[![appium project](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-1.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-1.png)

Then, write your project’s GroupId and ArtifactId. You can write the same as shown below. It does not affect anything, just naming.

[![project appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-2.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-2.png)

Then, give a name to your project.

[![test automation](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-3.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-3.png)

Then, click “Enable Auto-Import” in the right bottom corner.

[![mobile testing](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-enable-auto-import.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-enable-auto-import.png)

Go to mvnrepository.com website and get all frameworks’ latest dependency information. We will use **TestNG**, **Appium**, **Selenium**.

Our POM.xml will look like as below. You can see appium’s, selenium’s, and TestNG’s dependencies. I used JAVA11 JDK in this example.

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io-appium</groupId>
    <artifactId>io-appium</artifactId>
    <version>1.0-SNAPSHOT</version>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/io.appium/java-client -->
        <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>7.5.1</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>3.141.59</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.testng/testng -->
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.4.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>

Create a new Java Class as shown below and give a name to your test class. For now, keep it empty, after emulator settings, we will come back to test code at the end of the article.

[![android testing](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-6.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/appium-project-6.png)

Now, let’s start the emulator get your device’s information from the android studio as shown below.

[![android-virtual-device-selection](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-11.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-11.png)

After starting the application on the emulator, go to the server and click the magnifier icon to open the inspector.

[![appium-server-magnifier](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-13.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-13.png)

Then, start to enter the capabilities of your device as shown below in the inspector.

[![desired-capabilities-appium-inspector](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-14.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-14.png)

{
  "deviceName": "Pixel XL API 30",
  "platformName": "Android",
  "automationName": "UiAutomator2",
  "platformVersion": "11",
  "skipUnlock": "false"
}

Also, you can save these settings and use them later.

[![appium-desired-capabilities](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-15.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-15.png)

and click “Start Session” to start the inspector session to get your mobile elements ids. Get mobile element’s id’s as shown below. We will use them for our first mobile automation project.

[![appium-inspector](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-16.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-16.png)

After all of these steps, we need to write our test automation code.

## **How to Use Real Device instead of Emulator**

In emulators, you may face some problems. When I tried to run my test on the emulator XPath locators did not work. Normally, I did not want to use XPath but in our app some for some elements I did not have other options. Then, I tried the same test on a real device, my test worked flawlessly and very fast. Thus, I suggest you use real devices instead of emulators. In order to use the real device we should do the followings:

-   Connect your real device to your laptop via a USB port.
-   Go to Settings > Developer Settings and enable the USB Debugging option.
-   Open the command prompt and type “adb devices” command and get your device ID.

[![appium](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/adb-devices-command.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2018/01/adb-devices-command.png)

## **Mobile Test Automation Project by Using Appium and TestNG**

It is time to write some code for our Appium Tutorial. The test code of the project is shown below. I added inline comments. The most critical part is DesiredCapabilities, the rest of the code is very similar to Selenium & TestNG test automation codes. Also, you can find this project on GitHub.

> **Project’s GitHub URL: [https://github.com/swtestacademy/appium-sample-test](https://github.com/swtestacademy/appium-sample-test)**

Below code opens İsinOlsun app, skips splash screen, clicks “job search” button, then accepts notifications and then clicks the second job on the main screen and that’s all. :) It is easy because it is our first test case.

import io.appium.java\_client.MobileElement;
import io.appium.java\_client.android.AndroidDriver;
import java.net.MalformedURLException;
import java.net.URL;
import org.openqa.selenium.By;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class ioSampleTest {

    public AndroidDriver<MobileElement> driver;
    public WebDriverWait                wait;

    //Elements By
    By jobsBy           = By.id("com.isinolsun.app:id/rootRelativeView");
    By allowWhenUsingBy = By.id("com.android.permissioncontroller:id/permission\_allow\_foreground\_only\_button");
    By searchingJobBy   = By.id("com.isinolsun.app:id/bluecollar\_type\_button");
    By animationBy      = By.id("com.isinolsun.app:id/animation\_view");
    By toolBarTitleBy   = By.id("com.isinolsun.app:id/toolbarTitle");

    @BeforeMethod
    public void setup() throws MalformedURLException {
        DesiredCapabilities caps = new DesiredCapabilities();
        caps.setCapability("deviceName", "Pixel XL API 30");
        caps.setCapability("udid", "emulator-5554"); //DeviceId from "adb devices" command
        caps.setCapability("platformName", "Android");
        caps.setCapability("platformVersion", "11.0");
        caps.setCapability("skipUnlock", "true");
        caps.setCapability("appPackage", "com.isinolsun.app");
        caps.setCapability("appActivity", "com.isinolsun.app.activities.SplashActivity");
        caps.setCapability("noReset", "false");
        driver = new AndroidDriver<MobileElement>(new URL("http://127.0.0.1:4723/wd/hub"), caps);
        wait = new WebDriverWait(driver, 10);
    }

    @Test
    public void basicTest() throws InterruptedException {
        //Click and pass Splash
        wait.until(ExpectedConditions.visibilityOfElementLocated(animationBy)).click();

        //Click I am searching a job
        wait.until(ExpectedConditions.visibilityOfElementLocated(searchingJobBy)).click();

        //Notification Allow
        if (wait.until(ExpectedConditions.visibilityOfElementLocated(allowWhenUsingBy)).isDisplayed()) {
            wait.until(ExpectedConditions.visibilityOfElementLocated(allowWhenUsingBy)).click();
        }

        //Click Second Job
        wait.until(ExpectedConditions.visibilityOfAllElementsLocatedBy(jobsBy)).get(1).click();

        //Do a simple assertion
        String toolBarTitleStr = wait.until(ExpectedConditions.visibilityOfElementLocated(toolBarTitleBy)).getText();
        Assert.assertTrue(toolBarTitleStr.toLowerCase().contains("detay"));
    }

    @AfterMethod
    public void teardown() {
        driver.quit();
    }
}

Then, run the test.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-18.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-18.png)

And the test should pass as shown below screenshot.

[![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-19.png)](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2019/10/Pasted-into-Appium-Tutorial-Step-by-Step-Appium-Automation-19.png)

That’s all for this Appium Tutorial. I hope you successfully installed and configured all settings and run your mobile automation code.

If you have any questions please write a comment, me or another expert will help you.

## **GitHub Project**

**[https://github.com/swtestacademy/appium-sample-test](https://github.com/swtestacademy/appium-sample-test)**

## **Other Appium Tutorials**

**Now, you can learn Appium Parallel Testing and How to Setup your Own Wireless Mobile Device Farm!**

**[Appium Parallel Testing on Real Devices](http://www.swtestacademy.com/appium-parallel-tests/)**

**[Appium Parallel Testing on Multiple Emulators](https://www.swtestacademy.com/appium-parallel-tests-on-multiple-emulators/)**

**[Appium Cucumber TestNG with Parallel Test Execution](https://www.swtestacademy.com/appium-cucumber-testng-parallel-testing/)**

Do you want to learn **Appium Actions** such as **Tab**, **MultiTouch**, **Press**, **Swipe**?

**[Appium Mobile Actions](http://www.swtestacademy.com/appium-mobile-actions-swipe-tap-press/)**

I hope you like this article if you like it or if you have any troubles please share your comments with us. 

Thanks.  
Onur Baskirt

![](https://604223-1956433-raikfcquaxqncofqfm.stackpathdns.com/wp-content/uploads/2017/12/onur-baskirt-1-150x134.jpg)

Onur Baskirt is a senior IT professional with 15+ years of experience. Now, he is working as a Senior Technical Consultant at Emirates Airlines in Dubai.