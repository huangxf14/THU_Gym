#  @Albert Wu，Tsinghua University 2020-10-31
### -------------主要功能------------------------------------------------------------------------
# $\color{#FF3030}{记得安装testpackages.py文件中的packages}$
### 选择特定时间预定特定场次，如选择明天早上八点预约气膜馆羽2 20:30-22:30场次，预约之后15分钟内需要付款，当然可以选择pay way为2，为线下付款
### -----------------------------------------------------------------------------------------
### 气膜馆场次有两种情况，对应两批不同id，分别对应存储在气膜馆_多场地.csv及气膜馆_少场地.csv两个csv文件中
### -----------------------------------------------------------------------------------------
### 每天的场地时间都在变化，气膜馆应该只有两个类型，西体似乎有三个，并且不清楚即将开放预约的是哪一种类型，典型有以下两种类型：
![avatar](/pictures/class_1.png)
![avatar](/pictures/class_2.png)
### 所以也许会导致预约失败,对于气膜馆而言，建议将文件夹中对应的气膜馆_多场地.csv及气膜馆_少场地.csv中想要预定的id都添加到id.csv中，为了保险起见，建议第一列添加场地较多情况的id，第二列添加场地较少情况的id，依次交叉排布，保证任何一种情况都可以覆盖掉。

### id.csv排列情况如下：
### 如：id.csv，请勿覆盖掉列名称id
###  id
### 4929832
### 4839482
### 如果需要同时预约两个场地或者是一个场地的两个时段，请将想要预约的两个场地id，按照行排列一次加入id2.csv中
### 如：id2.csv,请勿在添加id时覆盖掉两个列名称id1,id2
### id1       id2
### 4080938 4829384
### 4728423 4582042
### 则程序会按照.csv文件中的顺序依次预约，比如首先尝试预约[4080938,4829384]，如果成功则停止，失败则继续预约[4728423,4582042]，直到成功或者尝试完id2.csv中的所有情况
### ------------------------------------------------------------------------------------------
# Version 0.3
### *增加了提交校内结算单
### ------------------------------------------------------------------------------------------
### 至主函数修改:main.py
### 文件夹中提供了气膜馆以及西体几种场地开放情况的场地对应的id，将想要预订的所有id复制到id.csv中，
### 注意不要覆盖id.csv第一行第一列的‘id’两个字母，所有id按行排列，则程序会按照id顺序依次进行预约，直到预约成功一个（当然，并不能同时预约西体和气膜馆的场地，建议选择气膜馆的场地）。
### 在main.py中initial parameters 部分修改用户名密码等参数，请勿修改login_gym和booking函数中参数！
### 倒数第二行，scheduler.add_job中修改hour 和minute，由于将登陆和预约分开运行（为了节约登陆花费的时间），所以将以下：
```python 
scheduler.add_job(login_gym, 'cron', day_of_week='*', hour=19, minute=1)
```
### 登陆对应的时间设置稍早于以下(早一分钟即可，早太多会导致登陆cookies失效）：
```python
 scheduler.add_job(booking, 'cron', day_of_week='*', hour=19, minute=2)
```
### houre为24小时制，如下午1点应该输入13
### ----------------------
# 运行：
### win+R 输入cmd 打开终端，cd 至本文件目录，执行python main.py，保持联网状态，则会在既定日期和时刻运行预订程序
