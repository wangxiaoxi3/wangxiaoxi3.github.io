---
layout: post
title: Selenium-显示等待方法
date: 2020-09-14 14:20:23 +0900
category: selenium
tag: selenium
---

### 1、显示等待语法
WebDriverWait(driver, timeout, poll_frequency, ignored_exceptions)
```
driver: 传入WebDriver实例，即webdriver.Chrome()
timeout: 超时时间，等待的最长时间（同时要考虑隐性等待时间）
poll_frequency: 调用until或until_not中的方法的间隔时间，默认是0.5秒
ignored_exceptions: 忽略的异常，如果在调用until或until_not的过程中抛出这个元组中的异常， 则不中断代码，继续等待，如果抛出的是这个元组外的异常，则中断代码，抛出异常。默认只有NoSuchElementException
```

### 2、until方法总结
隐式等待和显示等待都存在时，超时时间取二者中较大的

```
locator = (By.ID,'kw')

WebDriverWait(driver,10).until(EC.title_is(u"百度一下，你就知道"))
'''判断title,返回布尔值'''

WebDriverWait(driver,10).until(EC.title_contains(u"百度一下"))
'''判断title，返回布尔值'''

WebDriverWait(driver,10).until(EC.presence_of_element_located(locator))
'''判断某个元素是否被加到了dom树里，并不代表该元素一定可见，如果定位到就返回WebElement'''

WebDriverWait(driver,10).until(EC.visibility_of_element_located(locator))
'''判断某个元素是否被添加到了dom里并且可见，可见代表元素可显示且宽和高都大于0'''

WebDriverWait(driver,10).until(EC.visibility_of(driver.find_element(by=By.ID,value='kw')))
'''判断元素是否可见，如果可见就返回这个元素'''

WebDriverWait(driver,10).until(EC.presence_of_all_elements_located((By.CSS_SELECTOR,'.mnav')))
'''判断是否至少有1个元素存在于dom树中，如果定位到就返回列表'''

WebDriverWait(driver,10).until(EC.visibility_of_any_elements_located((By.CSS_SELECTOR,'.mnav')))
'''判断是否至少有一个元素在页面中可见，如果定位到就返回列表'''

WebDriverWait(driver,10).until(EC.text_to_be_present_in_element((By.XPATH,"//*[@id='u1']/a[8]"),u'设置'))
'''判断指定的元素中是否包含了预期的字符串，返回布尔值'''

WebDriverWait(driver,10).until(EC.text_to_be_present_in_element_value((By.CSS_SELECTOR,'#su'),u'百度一下'))
'''判断指定元素的属性值中是否包含了预期的字符串，返回布尔值'''

#WebDriverWait(driver,10).until(EC.frame_to_be_available_and_switch_to_it(locator))
'''判断该frame是否可以switch进去，如果可以的话，返回True并且switch进去，否则返回False'''
#注意这里并没有一个frame可以切换进去

WebDriverWait(driver,10).until(EC.invisibility_of_element_located((By.CSS_SELECTOR,'#swfEveryCookieWrap')))
'''判断某个元素在是否存在于dom或不可见,如果可见返回False,不可见返回这个元素'''
#注意#swfEveryCookieWrap在此页面中是一个隐藏的元素

WebDriverWait(driver,10).until(EC.element_to_be_clickable((By.XPATH,"//*[@id='u1']/a[8]"))).click()
'''判断某个元素中是否可见并且是enable的，代表可点击'''

#WebDriverWait(driver,10).until(EC.staleness_of(driver.find_element(By.ID,'su')))
'''等待某个元素从dom树中移除'''

WebDriverWait(driver,10).until(EC.element_to_be_selected(driver.find_element(By.XPATH,"//*[@id='nr']/option[1]")))
'''判断某个元素是否被选中了,一般用在下拉列表'''

WebDriverWait(driver,10).until(EC.element_selection_state_to_be(driver.find_element(By.XPATH,"//*[@id='nr']/option[1]"),True))
'''判断某个元素的选中状态是否符合预期'''

WebDriverWait(driver,10).until(EC.element_located_selection_state_to_be((By.XPATH,"//*[@id='nr']/option[1]"),True))
'''判断某个元素的选中状态是否符合预期'''

instance = WebDriverWait(driver,10).until(EC.alert_is_present())
'''判断页面上是否存在alert,如果有就切换到alert并返回alert的内容'''

```
