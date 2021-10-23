2021-06-11 16:48
#tag
# Appium
### [[Install Appium]]

#### [In project](https://github.com/appium/python-client)
```shell
pip install selenium
pip install appium-python-client
pip install behave

desired\_capabilities = {  
	'platformName': 'Android',  
	'platformVersion': '11',  
	'deviceName': 'Android Emulator',  
	'browserName': 'Chrome'  
}
```

[pip install Appium-Python-Client](https://pypi.org/project/Appium-Python-Client/)
```py
							"поиск элемента"
########### веб-приложениях WebElement 
searchBox=driver.findElement(By.id("lst-ib"))		# по id
	findElement(By.name(String Name))				# name
	findElement(By.linkText(String text))			# linkText imagesLink=driver.findElement(By.linkText("Images"))
	findElement(By.xpath(String XPath))				# XPath
	findElement(By.cssSelector(String cssSelector)	# css
########### поиск для нативных и гибридных приложений
'UIAutomatorviewer' #  в папке Android SDK (C:\\%android-sdk%\\tools)
	findElement(By.id(String id))
searchBox.sendKeys("Manoj Hans") #  ввести текст элемент (в поисковую строку)
```
### [Initial Setup](https://the-creative-tester.github.io/Python-Android-Mobile-Web-Automation/)
We are going to write our first automated test against [Google](http://www.google.com). By using the `unittest` framework, we can make use of the setUp() and tearDown() methods to define the initialization and cleanup for the fixture. You can run `adb devices -l` to get the model of your connected device, which should be used in the `deviceName` variable. If you are running with an emulated device, you may wish to change your `browserName` variable to use `'Browser'`, as emulated devices are not shippied with Chrome. With this information, you can now create a file, `appium_example.py` with the following contents:

```py
import unittest
from appium import webdriver

class AndroidMobileWebTest(unittest.TestCase):
    def setUp(self):
        desired_capabilities = {
            'platformName': 'Android',
            'platformVersion': '6.0',
            'deviceName': 'Nexus_5',
            'browserName': 'Chrome'
        }
        self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_capabilities)

    def test_mobileweb(self):
        self.driver.get('http://www.google.com')
        self.driver.find_element_by_name('q').clear()
        self.driver.find_element_by_name('q').send_keys('Appium')
        self.driver.find_element_by_name('q').submit()

    def tearDown(self):
        self.driver.quit()

```

_____________
#### Links
[! **__BOOK__** !](https://m.habr.com/ru/post/331714/)
[[Appium Tutorial]] | [[Mobile testing]] | [[Инструменты для тестировщика]]
[Appium python client’s documentation!](https://appium.github.io/python-client-sphinx/)
[Git](https://github.com/appium/python-client)
[Git python-client](https://github.com/appium/python-client)
[Appium Desired Capabilities](https://appium.io/docs/en/writing-running-appium/caps/)
 [Download RapidEE tool](https://www.rapidee.com/en/download) and install it and open it as administrator.