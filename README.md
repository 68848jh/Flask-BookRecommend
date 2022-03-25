------------------------------------------------------------------------------------------------
# 智能图书推荐系统                          
------------------------------------------------------------------------------------------------

数据集下载地址[http://www2.informatik.uni-freiburg.de/~cziegler/BX/](http://www2.informatik.uni-freiburg.de/~cziegler/BX/)

#### `主页`
<img src="./image/index.jpg" width="750" height="350">

#### `搜索功能`
<img src="./image/search.jpg" width="750" height="350">

#### `登录`
<img src="./image/login.jpg" width="750" height="350">

#### `注册`
<img src="./image/register.jpg" width="750" height="350">

#### `历史评分书单`
<img src="./image/historical.jpg" width="750" height="350">

#### `书单`
<img src="./image/userinfo.jpg" width="750" height="350">

#### `购物车`
<img src="./image/cart.jpg" width="750" height="350">

#### `管理员 用户删除`
<img src="./image/admin01.jpg" width="750" height="350">

#### `管理员 书籍添加删除`
<img src="./image/admin02.jpg" width="750" height="350">


*  对图书数据使用tensorflow和GPU加速实现了初版的协同过滤算法
（为了tensorflow的tensor运算，所以会创建比较大的矩阵，会初始化2个约27W乘10W的矩阵）

速度有比较大的提升。一天内可以训练完成。但是内存占用极高。接近42G内存。
所以在git上面CF4TensorFlow.py这个文件中第12行：    
```
Rating=Rating[:5000]   
```
设置了一个切片区间，默认使用5000，你可以按你的配置修改这个参数。
作者选择 Epoch 60000 Loss函数曲线 
<br>
<img src="./image/img7.png" width="350" height="250"><br>

### 功能清单

```
注册，登录，检索查询，评分，实时推荐，离线推荐，购物车，书单，删除购物车，删除书单。
管理员权限： 删除用户，添加书籍，删除书籍。
```

## 所需运行环境

* 使用python3.7作为编程语言。使用mysql作为数据库存储.
* 需要安装pandas,flask，pymysql.
*　安装方式:
```
    pip install pandas
    pip install flask
    pip install pymysql
```

## 项目启动方式：

数据集下载地址[http://www2.informatik.uni-freiburg.de/~cziegler/BX/]

* 将下载好的数据放入data文件夹下
* 运行read_data_save_to_mysql.py文件 将数据导入到mysql中。
  注意mysql的链接参数.默认是root,密码123456,端口是3300.如果你的不是，
  需要修改read_data_save_to_mysql和web/config.yml文件下的mysql的配置参数。
* 进入web文件夹,运行app.py
* 在浏览器上访问 127.0.0.1:8080  

* 使用下载数据中的UserID和其对应的Location作为账号密码登录网站。

* 系统管理员的账号：admin 密码：admin 通过这个账号密码进入后台管理

## 项目思路：

本项目实现了3个图书推荐功能：
+ 热门书籍 
    + 是将评分排名最高的几本书推荐给用户
+ 猜你喜欢
    + 通过数据库SQL语句实现
    + ”看了这本书的人也看了XX书“
    + 主要逻辑是：
        + 首先查该用户的浏览记录
        + 通过浏览过的书籍，找到也看过这本书的人
        + 在也看过这本书的人中，找评分较高的书推荐给用户
+ 推荐书籍
    + 离线计算好的推荐表的信息。使用到了协同过滤算法
    + 之后会做成按天更新
    + 目前的项目是实时推荐的，使用sql语句实现的
    
