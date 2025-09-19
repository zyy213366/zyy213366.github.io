---
title: 迪士尼门票计算代码分享
description: c++ 代码
date: 2024-11-26 13:48:02
tags: 代码分享
cover: /img/02.jpg
---
## 题目概述

![题目](img/disney.jpg)
根据题目我们可知，我们需要在了解入园观众信息后，对于票价进行计算。反馈给入园观众结果。

### 输入要求

1. **出生日期** : 用于计算买票人的年龄。

2. **身高** : 用于判断是否有特定优惠(例如儿童)。

3. **入园日期** : 用于区分平日或高峰日票价。


4. **票的类型** : 包括单日票和两日联票。
### 票价规则
1. **平日票价**：
   成人票：单日票370元，两日连票499元。
   儿童票（1.4米以下或65岁以上）：单日票280元，两日连票375元。
   青少年票（1.4米及以上且年龄不满14岁）：单日票280元，两日连票375元。
   婴幼儿（身高1米及以下）：免费。
2. **高峰日票价**：
   成人票：单日票499元，两日连票665元。
   儿童票（1.4米以下或65岁以上）：单日票375元，两日连票499元。
   青少年票（1.4米及以上且年龄不满14岁）：单日票375元，两日连票499元。
   婴幼儿（身高1米及以下）：免费。
## 代码演示
``` c++
//以下用c++代码实现
#include <iostream>
#include <string>
#include <ctime>
#include <cmath>

using namespace std;
int main() {
    int birthyear, birthmonth, birthday;
    int enteryear, entermonth, enterday;
    int height, tickettype;
    double baseprice;

    cout << "请输入出生年：";
    cin >> birthyear;
    cout << "请输入出生月：";
    cin >> birthmonth;
    cout << "请输入出生日：";
    cin >> birthday;
    cout << "请输入进入园区年份：";
    cin >> enteryear;
    cout << "请输入进入园区月份：";
    cin >> entermonth;
    cout << "请输入进入园区日期：";
    cin >> enterday;
    cout << "请输入身高(cm):";
    cin >> height;
    cout << "请输入票型(1.单日票 2.两日联票):";
    cin >> tickettype;

    // 处理基础票价
    if (enteryear == 2016 && entermonth == 6 && enterday >= 16 && enterday <= 30) {
        baseprice = 499;
    } else if (entermonth == 7 || entermonth == 8) {
        baseprice = 499;
    } else {
        tm timeStruct = {0};
        timeStruct.tm_year = enteryear - 1900;
        timeStruct.tm_mon = entermonth - 1;
        timeStruct.tm_mday = enterday;
        mktime(&timeStruct);
        int weekday = timeStruct.tm_wday;
        if (weekday == 0 || weekday == 6) {
            baseprice = 499;
        } else {
            baseprice = 370;
        }
    }
    // 计算节假日
    if (entermonth == 10 && enterday >= 1 && enterday <= 7 || entermonth == 12 && enterday == 27)
    {
        baseprice = 499;
    }
    if (entermonth == 1 && enterday == 1 || entermonth == 5 && enterday == 1)
    {
        baseprice = 499;
    }
    if (entermonth == 9 && enterday == 10)
    {
        baseprice = 499;
    }
    

    // 计算年龄
    double age = enteryear - birthyear + (entermonth - birthmonth) / 12.0 + (enterday - birthday) / 365.0;

    // 计算折扣
    double discount1;
    if (height > 140 && age < 65) {
        discount1 = 1;
    } else if (height > 100 && height <= 140 || age >= 65) {
        discount1 = 0.75;
    } else if (height <= 100) {
        discount1 = 0;
    } else {
        discount1 = 1; // 默认不打折
    }

    double discount2;
    if (tickettype == 1) {
        discount2 = 1;
    } else if (tickettype == 2) {
        discount2 = 2*0.95;
    } else {
        discount2 = 1; // 默认票型1
    }
    double midprice = discount1 *baseprice;
    double finalprice;
    if (midprice >= 300){
        finalprice= discount2 * 375;
    }
    if (midprice ==0){
        finalprice=0;
    }
    else{
        finalprice= discount2 * 280;
    }
    cout << "最终票价: " << finalprice << endl;

    return 0;
}
```
如果觉得写的不错的话可以打赏来支持哦~~
![](/img/下载1.jpg#pic_left)
<style>
    img[alt='办公室合影']{
    max-width: 1%;
    }
</style>
