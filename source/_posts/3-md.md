---
title: 自动抢课脚本的python实现
date: 2024-12-23 22:31:40
tags: 代码分享
cover: /img/02.jpg
---
## 自动抢课脚本的python实现
### 缘起
我们学校的抢课是按照绩点抢课，实际上没有拼手速的需求。但是我的一位在工技大的好友急切地需要抢课的脚本，这也是我写python自动抢课脚本的最大动力。
### 思路
这并非是我的原创，我是依据同学所创上大抢课脚本改编而成。
1. 首先我们需要引入```selenium```和```apscheduler```两个库来完成网页打开和其中的html解析。通过实例化窗口进入学校抢课的官网，用户在窗口完成登录。
2. 我通过```find_element```方法找到选课按钮所在的```x_path```，运用之前导入的库完成网页的跳转（我们不能直接在一开始就进入选课网页的原因是缺乏了登陆的步骤）。
3. 之后则如法炮制，我提取课程号和教师号输入框的```x_path```，通过```send_keys```方法完成内容的填充，最后提交即可。
   **注：** 由于过快的提交有被ban的风险，因此在每节课输入后适当加入了```sleep```方法减慢速度，不过总体上对于选课来说依然绰绰有余。
### 代码
```python
'''
使用方法：
确保安装了selenium和apscheduler,没有安装自行pip
在courses_and_teachers中事先填好课程号和教师号，修改start_time
注意不要调整窗口大小
启动脚本，先登录选好学期，直到进入选课界面为止 然后在控制台按回车（这一步是确认你登录了）
控制台说任务已安排，等待即可
'''

from selenium import webdriver
from selenium.webdriver.common.by import By
import time
from apscheduler.schedulers.background import BackgroundScheduler

# 创建浏览器实例并设置窗口大小
driver = webdriver.Chrome()
driver.set_window_size(800, 900)  # 设置窗口大小为800x900


start_time = '2024-12-26 12:50:01'  # 设置为你的目标时间


def fill_courses():
    try:
        # 第一节课
        # 使用 XPath 找到按钮
        button = driver.find_element(By.XPATH, '//*[@id="moduleId120446"]/div/tr[3]/td[8]/button/span')
        # 点击按钮
        button.click()
        time.sleep(1)
            # 填充课程号的输入框
        course_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[1]/div[2]/input'
        course_input = driver.find_element(By.XPATH, course_input_xpath)
        course_input.clear()
        course_input.send_keys('229202')

            # 填充教师号的输入框
        teacher_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[2]/div[2]/input'
        teacher_input = driver.find_element(By.XPATH, teacher_input_xpath)
        teacher_input.clear()
        teacher_input.send_keys('0597')

        # 点击第一个查询按钮
        first_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[16]/button[1]/span')
        first_confirm_button.click()
        time.sleep(1)

        # 点击第二个确认按钮
        second_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[3]/div/div[1]/div[3]/table/tbody/tr/td[7]/div/button/span')
        second_confirm_button.click()
        
        time.sleep(2)
        try:
            close_button1 = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[4]/div/div[3]/span/button/span')
            close_button1.click()
        except:
            print(1)
        close_button2 = driver.find_element(By.XPATH, '//*[@id="el-drawer__title"]/button')
        close_button2.click()

        # 第二节课
        # 使用 XPath 找到按钮
        button = driver.find_element(By.XPATH, '//*[@id="moduleId138994"]/div/tr[1]/td[8]/button/span')
        # 点击按钮
        button.click()
        time.sleep(1)
            # 填充课程号的输入框
        course_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[1]/div[2]/input'
        course_input = driver.find_element(By.XPATH, course_input_xpath)
        course_input.clear()
        course_input.send_keys('180202') 

            # 填充教师号的输入框
        teacher_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[2]/div[2]/input'
        teacher_input = driver.find_element(By.XPATH, teacher_input_xpath)
        teacher_input.clear()
        teacher_input.send_keys('1308') #可修改引号中的课程

        # 点击第一个查询按钮                                  
        first_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[16]/button[1]/span')
        first_confirm_button.click()
        time.sleep(1)

        # 点击第二个确认按钮
        second_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[3]/div/div[1]/div[3]/table/tbody/tr/td[7]/div/button/span')
        second_confirm_button.click()
        
        time.sleep(2)
        try:
            close_button1 = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[4]/div/div[3]/span/button/span')
            close_button1.click()
        except:
            print(1)
        close_button2 = driver.find_element(By.XPATH, '//*[@id="el-drawer__title"]/button')
        close_button2.click()

        # 第三节课
        # 使用 XPath 找到按钮
        button = driver.find_element(By.XPATH, '//*[@id="moduleId138994"]/div/tr[2]/td[8]/button/span')
        # 点击按钮
        button.click()
        time.sleep(1)
            # 填充课程号的输入框
        course_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[1]/div[2]/input'
        course_input = driver.find_element(By.XPATH, course_input_xpath)
        course_input.clear()
        course_input.send_keys('180204')

            # 填充教师号的输入框
        teacher_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[2]/div[2]/input'
        teacher_input = driver.find_element(By.XPATH, teacher_input_xpath)
        teacher_input.clear()
        teacher_input.send_keys('1633')

        # 点击第一个查询按钮
        first_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[16]/button[1]/span')
        first_confirm_button.click()
        time.sleep(1)

        # 点击第二个确认按钮
        second_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[3]/div/div[1]/div[3]/table/tbody/tr/td[7]/div/button/span')
        second_confirm_button.click()
        
        time.sleep(2)
        try:
            close_button1 = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[4]/div/div[3]/span/button/span')
            close_button1.click()
        except:
            print(1)
        close_button2 = driver.find_element(By.XPATH, '//*[@id="el-drawer__title"]/button')
        close_button2.click()
        time.sleep(2)

        # 第四节课
        # 使用 XPath 找到按钮
        button = driver.find_element(By.XPATH, '//*[@id="moduleId120459"]/div/tr[5]/td[8]/button/span')
        # 点击按钮
        button.click()
        time.sleep(1)
            # 填充课程号的输入框
        course_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[1]/div[2]/input'
        course_input = driver.find_element(By.XPATH, course_input_xpath)
        course_input.clear()
        course_input.send_keys('219261')

            # 填充教师号的输入框
        teacher_input_xpath = f'//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[2]/div[2]/input'
        teacher_input = driver.find_element(By.XPATH, teacher_input_xpath)
        teacher_input.clear()
        teacher_input.send_keys('0156')

        # 点击第一个查询按钮
        first_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[1]/form/div/div[16]/button[1]/span')
        first_confirm_button.click()
        time.sleep(1)

        # 点击第二个确认按钮
        second_confirm_button = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[3]/div/div[1]/div[3]/table/tbody/tr/td[7]/div/button/span')
        second_confirm_button.click()
       
        time.sleep(2)
        try:
            close_button1 = driver.find_element(By.XPATH, '//*[@id="programs"]/div[3]/div[1]/div/div/section/div/div/div/div[4]/div/div[3]/span/button/span')
            close_button1.click()
        except:
            print(1)
        close_button2 = driver.find_element(By.XPATH, '//*[@id="el-drawer__title"]/button')
        close_button2.click()


        
        
    except Exception as e:
        print(f"出现错误: {e}")


# 创建调度器
scheduler = BackgroundScheduler()

# 设置定时任务，指定开始时间

scheduler.add_job(fill_courses, 'date', run_date=start_time)


try:
    # 访问目标网页
    driver.get('https://jxfw.sues.edu.cn/student/home')
    print("请手动登录（在新的标签页中），登录完成后按 Enter 键继续...")

    # 等待用户手动登录
    input("按 Enter 键继续...")
    scheduler.start()
    # 等待网页加载
    time.sleep(2)
    driver.get('https://jxfw.sues.edu.cn/course-selection/?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJzdXB3aXNkb20iLCJleHAiOjE3MzQ5OTM1MzYsInVzZXJuYW1lIjoiMDI4MTI0NDYxIn0.A_OCgI0vnuc6iKe1LXmee_lrOKoCNycZeWj1HgdNyQI#/course-select/turns')
    time.sleep(2)

    driver.get('https://jxfw.sues.edu.cn/course-selection/#/course-select/72188805/turn/450/select')
    
    time.sleep(2)
    # 启动调度器
    
    print("任务已安排，将在指定时间执行...")

    # 保持主线程运行
    while True:
        time.sleep(1)

finally:
    # 关闭浏览器和调度器
    scheduler.shutdown()
    driver.quit()

```

