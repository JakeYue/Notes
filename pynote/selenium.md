## Selenium

> 使用 selenium 需要先安装 浏览器驱动，selenium 支持多种浏览器.[chromedriver](http://chromedriver.storage.googleapis.com/index.html)

### Selenium 各种类

---

#### Webdriver.common

`selenium.webdriver.common.action_chains`:(行动链)(鼠标键盘)

`.alert`:警告实施

`.by`:BY实现

`.desired_capabilities`:所需功能实现

`.common.keys` :Keys 实现。
`.touch_actions` :触摸操作实现
`.utils`: Utils 方法。
`.proxy` :代理实现。
`.service`
`.html5.application_cache` : 应用程序缓存 实现。

#### Webdriver.support
`selenium.webdriver.support.abstract_event_listener`
`.color`
`.event_firing_webdriver`
`.expected_conditions`
`.select`
`.wait`

#### Webdriver.chrome 网络驱动程序

`selenium.webdriver.chrome.options
selenium.webdriver.chrome.service
selenium.webdriver.chrome.webdriver`

---

####  Webdriver.chromium

`selenium.webdriver.chromium.options
selenium.webdriver.chromium.service
selenium.webdriver.chromium.webdriver`

#### Webdriver.edge
`selenium.webdriver.edge.options
selenium.webdriver.edge.service
selenium.webdriver.edge.webdriver`

#### webdriver.firefox
`selenium.webdriver.firefox.extension_connection
selenium.webdriver.firefox.firefox_binary
selenium.webdriver.firefox.options
selenium.webdriver.firefox.firefox_profile
selenium.webdriver.firefox.webdriver`

#### Webdriver.ie
`selenium.webdriver.ie.webdriver`	

#### Webdriver.opera
`selenium.webdriver.opera.webdriver`

#### Webdriver.remote

`selenium.webdriver.remote.command`
`.errorhandler`
`.remote.mobile`
`.remote_connection`
`.remote.utils`
`.webdriver Webdriver` WebDriver实现
`.webelement`

#### Webdriver.safari
`selenium.webdriver.safari.service
selenium.webdriver.safari.webdriver`

#### Webdriver.webkitgtk

`selenium.webdriver.webkitgtk.options
selenium.webdriver.webkitgtk.service
selenium.webdriver.webkitgtk.webdriver`

---

#### 常用 import 导入

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.select import Select
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
```

### 常用方法

---

#### 构造模拟浏览器

`browser = webdriver.Chrome('driver/path')`

```python
# 这是设定无界面模式,即隐藏浏览器
options = webdriver.ChromeOptions('driver/path')
options.add_argument('--headless')
browser = webdriver.Chrome(chrome_options=options)
```

#### 访问页面

```python
# 用get方法访问页面
browser.get('url')
# 下面可选:
firefox_login.maximize_window()　　# 窗口最大化
firefox_login.minimize_window()    #最小窗口
```

#### 查找元素并交互

```python
# 通过id查找
browser.find_element_by_id('email').clear()
browser.firefox_login.find_element_by_id('email').send_keys('xxx@sina.com')

# 元素查找方法:
# 通过名称查找
find_element_by_name 
# 通过id查找
find_element_by_id  
# 通过xpath查找
find_element_by_xpath
# 通过链接文本获取指定的一个超链接
find_element_by_link_text
# 通过链接文本获取指定的一个超链接(模糊匹配)
find_element_by_partial_link_text
# 通过标签查找
find_element_by_tag_name
# 通过class名称查找
find_element_by_class_name
# 通过CSS选择器查找
find_element_by_css_selector

'''上面是单元素查找,多元素查找把element变成elements'''
find_elements_by_xpath
```

#### 还有比较通用的方法

```python
# 导入by类,By类方法
from selenium.webdriver.common.by import By
# 构造浏览器对象
browser = webdriver.Chrome()
browser.get("http://www.taobao.com")
#查找元素,其中By.ID的ID可以换成别的,如xpath
input_first = browser.find_element(By.ID,"q") 

# 2个私有方法
browser.find_element()
browser.find_elements()

#  By类可用属性
ID = "id"
XPATH = "xpath"
LINK_TEXT = "link text"
PARTIAL_LINK_TEXT = "partial link text"
NAME = "name"
TAG_NAME = "tag_name"
CLASS_NAME = "class name"
CSS_SELECTOR = "css selector"
```

---

#### 操作浏览器动作

> 行为链类,`ActionChains`可以完成简单的交互行为，例如鼠标移动，鼠标点击事件，键盘输入以及内容菜单交互.复杂的类似于鼠标悬停和拖拽行为.
>
> `class selenium.webdriver.common.action_chains.ActionChains(driver)`

当你在`ActionChains`对象上调用行为方法时，这些行为会存储在`ActionChains`对象的一个队列里。调用`perform()`时，这些动作就以他们队列的顺序来触发.

**鼠标、键盘行为**

```python
# 点击一个元素
click(on_element=None) # 参数：* on_element:要点击的元素，如果是None，点击鼠标当前的位置
# 鼠标左键点击一个元素并且保持
click_and_hold(on_element=None)
# 双击一个元素
double_click(on_element=None)

# 鼠标左键点击source元素，然后移动到target元素释放鼠标按键
drag_and_drop(source, target) # 参数： source:鼠标点击的元素 target:鼠标松开的元素
# 拖拽目标元素到指定的偏移点释放
drag_and_drop_by_offset(source, xoffset,yoffset) # 参数: source:点击的参数 xoffset:X偏移量 * yoffset:Y偏移量
# 只按下键盘，不释放;我们应该只对那些功能键使用(Contril,Alt,Shift)
key_down(value,element=None) # 参数：value：要发送的键，值在Keys类里有定义 element:发送的目标元素，如果是None，value会发到当前聚焦的元素上
# 释放键; 参数同上
key_up(value,element=None)
# 将当前鼠标的位置进行移动
move_by_offset(xoffset,yoffset) # xoffset:要移动的X偏移量，可以是正也可以是负 yoffset:要移动的Y偏移量，可以是正也可以是负
# 把鼠标移到一个元素的中间
move_to_element(to_element)
# 鼠标移动到元素的指定位置，偏移量以元素的左上角为基准
move_to_element_with_offset(to_element,xoffset,yoffset)
# 执行所有存储的动作
perform()
# 释放一个元素上的鼠标按键
release(on_element=None)
# 向当前的焦点元素发送键
send_keys(*keys_to_send)
# 向指定的元素发送键
send_keys_to_element(element,*keys_to_send)
```

**常见浏览器操作方法**

```python
# 设置浏览器的大小
set_window_size()
# 控制浏览器后退
back()
# 控制浏览器前进
forward()
# 刷新当前页面
refresh()
# 清除文本
clear()
# 模拟按键输入
send_keys (value)
# 单击元素
click()
# 用于提交表单
submit()
# 获取元素属性值
get_attribute(name)
# 设置该元素是否用户可见
is_displayed()
# 返回元素的尺寸
size
# 获取元素文本
text
# 获取本页面URL
c.current_url
# 获取日志
c.log_types  #获取当前日志类型
c.get_log('browser')#浏览器操作日志
c.get_log('driver') #设备日志
c.get_log('client') #客户端日志
c.get_log('server') #服务端日志

# 窗口操作
c.maximize_window()#最大化
c.fullscreen_window() #全屏
c.minimize_window() #最小化
c.get_window_position() #获取窗口的坐标
c.get_window_rect()#获取窗口的大小和坐标
c.get_window_size()#获取窗口的大小
c.set_window_position(100,200)#设置窗口的坐标
c.set_window_rect(100,200,32,50)    #设置窗口的大小和坐标
c.set_window_size(400,600)#设置窗口的大小
c.current_window_handle   #返回当前窗口的句柄
c.window_handles         #返回当前会话中的所有窗口的句柄

# 设置延时
c.set_script_timeout(5) #设置脚本延时五秒后执行
c.set_page_load_timeout(5)#设置页面读取时间延时五秒

# 关闭
c.close() #关闭当前标签页
c.quit() #关闭浏览器并关闭驱动
# 打印网页源代码
c.page_source

# 屏幕截图操作
c.save_screenshot('1.png')#截图，只支持PNG格式
c.get_screenshot_as_png() #获取当前窗口的截图作为二进制数据
c.get_screenshot_as_base64() #获取当前窗口的截图作为base64编码的字符串

# 前进后退刷新
c.forward() #前进
c.back()  #后退
c.refresh()#刷新
```

**元素操作**

```python
kw1.id             #Selenium所使用的内部ID
kw1.get_property('background') #获取元素的属性的值
kw1.get_attribute('id') #获取元素的属性的值
kw1.location       #不滚动获取元素的坐标
kw1.location_once_scrolled_into_view  #不滚动且底部对齐并获取元素的坐标
kw1.parent         #父元素
kw1.screenshot('2.png') #截取元素形状并保存为图片
kw1.tag_name       #标签名
kw1.text           #内容，如果是表单元素则无法获取
kw1.is_selected()  #判断元素是否被选中
kw1.is_enabled()   #判断元素是否可编辑
kw1.is_displayed   #判断元素是否显示
kw1.value_of_css_property('color') #获取元素属性的值
kw1._upload('2.png') #上传文件

# 获取标题内容
c.title

# 获取当前浏览器名
c.name

# 全局超时时间
c.implicitly_wait(5)
# 上传文件: 定位上传按钮，添加本地文件
driver.find_element_by_name("file").send_keys('localfile') 
```

---

#### 选项操作(options)

`from selenium.webdriver import ChromeOptions`

```python
o.add_argument('--window-size=600,600') #设置窗口大小
o.add_argument('--incognito') #无痕模式
o.add_argument('--disable-infobars') #去掉chrome正受到自动测试软件的控制的提示
o.add_argument('user-agent="XXXX"') #添加请求头
o.add_argument("--proxy-server=http://200.130.123.43:3456")#代理服务器访问
o.add_experimental_option('excludeSwitches', ['enable-automation'])#开发者模式
o.add_experimental_option("prefs",{"profile.managed_default_content_settings.images": 2})#禁止加载图片
o.add_experimental_option('prefs',
{'profile.default_content_setting_values':{'notifications':2}}) #禁用浏览器弹窗
o.add_argument('blink-settings=imagesEnabled=false')  #禁止加载图片
o.add_argument('lang=zh_CN.UTF-8') #设置默认编码为utf-8
o.add_extension(create_proxyauth_extension(
           proxy_host='host',
           proxy_port='port',
           proxy_username="username",
           proxy_password="password"
       ))# 设置有账号密码的代理
o.add_argument('--disable-gpu')  # 这个属性可以规避谷歌的部分bug
o.add_argument('--disable-javascript')  # 禁用javascript
o.add_argument('--hide-scrollbars')  # 隐藏滚动条
o.binary_location=r"C:\Users\Administrator\AppData\Local\Google\Chrome\Application" #指定浏览器位置
o.add_argument('--no-sandbox')  #解决DevToolsActivePort文件不存在的报错

##############################

# 常用的几种
o.set_headless()          #设置启动无界面化
o.binary_location(value)  #设置chrome二进制文件位置
o.add_argument(arg)               #添加启动参数
o.add_extension(path)                #添加指定路径下的扩展应用
o.add_encoded_extension(base64)      #添加经过Base64编码的扩展应用
o.add_experimental_option(name,value)         #添加实验性质的选项
o.debugger_address(value)                #设置调试器地址 
o.to_capabilities()                    #获取当前浏览器的所有信息
```

#### 下拉框处理

> selenium提供了select了来处理下拉框,select存在WebDriver中

- select类=> `from selenium.webdriver.support.select import Select`(引入select)

```python
# 用于选取< option>标签的value值，要求必须要有value属性;
select_by_value()

# 要求下拉框的选项必须要有index属性,例如index=”1”,数字从0开始;
select_by_index()

# 用于选取< option>标签的 text 值.
select_by_visible_text

# 选择第一个option 选项
first_selected_option(self) 
```

```python
# 清楚选择列表(下拉框)选项
# 用于选取< option>标签的value值，要求必须要有value属性;
deselect_by_value()

# 要求下拉框的选项必须要有index属性,例如index=”1”;
deselect_by_index()

# 用于选取< option>标签的 text 值.
deselect_by_visible_text

#清楚所有
deselect_all(self)

```

#### 对话框

> 浏览器本身弹出对话框主要分为三种类型：“警告消息框”，“确认消息框”，“提示消息对话”三种类型的对话框.
>
> 1.警告消息框（alert）只有确定
>
> 2.确认消息框（confirm）确定/取消
>
> 3.提示消息对话（prompt）确定/取消;输入框

**selenium提供switch_to_alert()方法定位到alert/confirm/prompt对话框**

```python
# 用find_方法或其他方法定位到元素,点击元素出现对话框
switch_to.alert # 定位到警告框
text # 获取警告框中的文字信息
accept() # 接受现有警告框(相当于确认)
dismiss() # 解散现有警告框(相当于取消)
send_keys('文本内容') # 发送文本至警告框(适用于有输入对话框的弹窗)

# 其他html内的对话框使用正常方法就可以
```

#### 界面冻结

> 在开发者工具console选项里面输入代码:`setTimeout(function(){debugger},5000)`;表示在5秒后启动debug,debug会冻住界面

----

#### (Frame/IFrame)切换/窗口操作

> 1. 页面中有时会嵌套html,selenium操作范围只在缺省默认页面,需要切换到对应的范围操作.
>
> 2. frame标签有frameset、frame、iframe三种，frameset跟其他普通标签没有区别
> 3. frame、iframe需要切换,selenium有一组方法对frame进行操作.

```python
# 切入子范围
swich_to.frame(refrence) #reference是传入的参数，用来定位frame，可以传入id、name、index、xpath以及selenium的WebElement对象;
#WebElement对象:就是用find_element_by_**(iframe元素)传入swich_to.frame
# 切回主范围
swich_to.default_content()
# 嵌套frame的操作
# 多层切入要一层一层进入
swich_to.frame(refrence1)
swich_to.frame(refrence2)
# 回退
switch_to.parent_frame() # 如果时主范围则无效
```

**切换窗口/新标签页**

```python
# 获取所有窗口的句柄
window_handles
# 切换窗口
switch_to.window()
#切换到最新打开的窗口
switch_to.window(windows[-1])
# 获得打开的当前窗口的句柄
window1 = current_window_handle
# 获取所有窗口的句柄
windows = window_handles()
# for 循环窗口句柄,使用窗口title判断是否是要进入的窗口
for current_window in windows:
    if "title" in current_window:
        dir.switch_to.window(current_window)
        
# 如果只有2个窗口也可以直接判断
for current_window in windows:
    if window1 != current_window:
        dir.switch_to.window(current_window)
# 回到开始的窗口;使用记录的句柄返回
switch_to.window(window1)

```

---

#### 设置等待

> 如果遇到使用ajax加载的网页，页面元素可能不是同时加载出来的，这个时候，就需要我们通过设置一个等待条件，等待页面元素加载完成，避免出现因为元素未加载导致的错误的出现

**WebDriver提供了两种等待类型：显示等待、隐式等待**

 *1.显示等待：WebDriverWait()类*

- 显示等待：设置一个等待时间和一个条件，在规定时间内，每隔一段时间查看下条件是否成立，如果成立那么程序就继续执行，否则就提示一个超时异常（TimeoutException）
- 通常情况下**WebDriverWait**类会结合**ExpectedCondition**类一起使用

> WebDriverWait的具体参数和方法：

```python
WebDriverWait(driver,timeout,poll_frequency=0.5,ignored_exceptions=None)
'''  
driver: 浏览器驱动
timeout: 超时时间，等待的最长时间（同时要考虑隐性等待时间）
poll_frequency: 每次检测的间隔时间，默认是0.5秒
ignored_exceptions:超时后的异常信息，默认情况下抛出NoSuchElementException异常
'''

until(method,message='')
'''
    method: 在等待期间，每隔一段时间调用这个传入的方法，直到返回值不是False
    message: 如果超时，抛出TimeoutException，将message传入异常
'''

until_not(method,message='')
'''
until_not 与until相反，until是当某元素出现或什么条件成立则继续执行，
until_not是当某元素消失或什么条件不成立则继续执行，参数也相同。
method
message
'''
```

> ExpectedCondition中可使用的判断条件:

```python
from selenium.webdriver.support import expected_conditions as EC

# 判断标题是否和预期的一致
title_is
# 判断标题中是否包含预期的字符串
title_contains

# 判断指定元素是否加载出来
presence_of_element_located
# 判断所有元素是否加载完成
presence_of_all_elements_located

# 判断某个元素是否可见. 可见代表元素非隐藏，并且元素的宽和高都不等于0，传入参数是元组类型的locator
visibility_of_element_located
# 判断元素是否可见，传入参数是定位后的元素WebElement
visibility_of
# 判断某个元素是否不可见，或是否不存在于DOM树
invisibility_of_element_located


# 判断元素的 text 是否包含预期字符串
text_to_be_present_in_element
# 判断元素的 value 是否包含预期字符串
text_to_be_present_in_element_value


#判断frame是否可切入，可传入locator元组或者直接传入定位方式：id、name、index或WebElement
frame_to_be_available_and_switch_to_it


#判断是否有alert出现
alert_is_present


#判断元素是否可点击
element_to_be_clickable


# 判断元素是否被选中,一般用在下拉列表，传入WebElement对象
element_to_be_selected
# 判断元素是否被选中
element_located_to_be_selected
# 判断元素的选中状态是否和预期一致，传入参数：定位后的元素，相等返回True，否则返回False
element_selection_state_to_be
# 判断元素的选中状态是否和预期一致，传入参数：元素的定位，相等返回True，否则返回False
element_located_selection_state_to_be


#判断一个元素是否仍在DOM中，传入WebElement对象，可以判断页面是否刷新了
staleness_of
```

**调用方法:**

`WebDriverWait(driver, 超时时长, 调用频率, 忽略异常).until(可执行方法, 超时时返回的信息)`

 **2.隐式等待**

- implicitly_wait(xx)：设置等待时间为xx秒，等待元素加载完成，如果到了时间元素没有加载出，就抛出一个NoSuchElementException的错误。
- 注意：隐性等待对整个driver的周期都起作用，所以只要设置一次即可

`driver.implicitly_wait(30)  # 隐性等待，最长等30秒`

**强制等待：sleep()**

- 调用time的sleep方法
- 强制等待：不管浏览器元素是否加载完成，程序都得等待3秒，3秒一到，继续执行下面的代码。

---

#### 调用javascript代码

**execute_script()方法来执行JavaScript代码**

```javascript
# 用于设置浏览器滚动条位置的JavaScript代码:
<!-- window.scrollTo(左边距,上边距); -- 
window.scrollTo(0,450);
```

```python
from selenium import webdriver
from time import sleep

# 1.访问百度
drive = webdriver.Chrome(executable_path='chromedriver.exe')
drive.get('https://www.baidu.com')

# 2.搜索
drive.find_element_by_id('kw').send_keys('python')
drive.find_element_by_id('su').click()

# 3.休眠2s,获取服务器的响应内容
sleep(2)

# 4.通过javascript设置浏览器窗口的滚动条位置
drive.execute_script('window.scrollTo(0, 500)')
# drive.execute_script('window.scrollTo(0, document.body.scrollHeight)') #滑到最底部

sleep(2)
drive.close()

# 执行js，让滚轮向下滚动
bor.execute_script('window.scrollTo(0, document.body.scrollHeight)')
sleep(2)
```

---

#### **page_source**用于获取页面源码数据

```python
from selenium import webdriver
from time import sleep

# 1.访问百度
drive = webdriver.Chrome(executable_path='chromedriver.exe')
drive.get('https://www.baidu.com')

# 2.搜索
drive.find_element_by_id('kw').send_keys('python')
drive.find_element_by_id('su').click()

# 3.休眠2s,获取服务器的响应内容
sleep(2)

# 4.获取页面源码数据
text = drive.page_source
print(text)

drive.close()
```

---

#### **cookie操作**

> 有时候我们需要验证浏览器中cookie是否正确，因为基于真实cookie的测试是无法通过白盒和集成测试进行的。WebDriver提供了操作Cookie的相关方法，可以读取、添加和删除cookie信息

```python
# 获得所有cookie信息
get_cookies()
# 返回字典的key为“name”的cookie信息
get_cookie(name)
# 添加cookie。“cookie_dict”指字典对象，必须有name 和value 值
add_cookie(cookie_dict)
# 删除cookie信息。“name”是要删除的cookie的名称，“optionsString”是该cookie的选项，目前支持的选项包括“路径”，“域”
delete_cookie(name,optionsString)
# 删除所有cookie信息
delete_all_cookies()

```

```python
from selenium import webdriver
drive = webdriver.Chrome(executable_path='chromedriver.exe')
drive.get('https://www.cnblogs.com/')

# 1.打印cookie信息
print(drive.get_cookies())

# 2.添加cookie信息
dic = {'name':'name', 'value':'python'}
drive.add_cookie(dic)
print(drive.get_cookies())

# 3.遍历打印cookie信息
for cookie in drive.get_cookies():
 print(f"{cookie['name']}---f{cookie['value']}\n")

drive.close()
```

---

#### **selenium规避被检测识别**

> 只需要设置Chromedriver的启动参数即可解决问题,在启动Chromedriver之前，为Chrome开启实验性功能参数`excludeSwitches`，它的值为`['enable-automation']`

```python
from selenium import webdriver
from selenium.webdriver import ChromeOptions

# 1.实例化一个ChromeOptions对象
option = ChromeOptions()
option.add_experimental_option('excludeSwitches', ['enable-automation'])

# 2.将ChromeOptions实例化的对象option作为参数传给Crhome对象
driver = webdriver.Chrome(executable_path='chromedriver.exe', options=option)

# 3.发起请求
driver.get('https://www.taobao.com/')
```

### 登录edge方法

用`selenium` 登录新版的`edge`方法

首先，安装`pip install msedge-selenium-tools`

接下来，使用以下代码：

```python
from selenium import webdriver
from msedge.selenium_tools import Edge, EdgeOptions
options = EdgeOptions()
options.use_chromium = True
options.binary_location = r"C:\xx\Microsoft\EdgeCore\93.0.926.0\msedge.exe" # 浏览器的位置
driver = Edge(options=options, executable_path=r"D:\xx\Desktop\edgedriver_win64\msedgedriver.exe") # 相应的浏览器的驱动位置

driver.get("www.baidu.com")

```
