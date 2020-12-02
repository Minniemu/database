
Database 建置過程中遇到的問題
===


[![hackmd-github-sync-badge](https://hackmd.io/ya1j_sHuT9eWIMde7A2ipg/badge)](https://hackmd.io/ya1j_sHuT9eWIMde7A2ipg)


## Table of Contents

[TOC]

## 2020.12.01

1. 今天上線就發現我的資料庫好像掛掉了?
> Error message : Mysql連線的過程中出現Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock

```gherkin=
student02@student02:~$ mysql
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld
student02@student02:~$ sudo service mysql start
[sudo] password for student02:
 * Starting MySQL database server mysqld                                        
 No directory, logging in with HOME=/
```

2. Solution : 
> (1) 刪除 mysql : 
```gherkin=
sudo apt-get autoremove --purge mysql-server-5.0
sudo apt-get remove mysql-server
sudo apt-get autoremove mysql-server
sudo apt-get remove mysql-common //這個很重要上面的其實有一些是多餘的。
```
> (2) 清理殘留資料 :
```gherkin=
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
``` 


## MySQL on Ubuntu

1. 安裝 mysql
```gherkin=
sudo apt-get install mysql-server
sudo apt-get install mysql-client
sudo apt-get install libmysqlclient-dev
sudo apt-get install net-tools
```
2. 檢查預設的MySQL使用者帳號及密碼
> 在安裝完成後，接下來我們要做的便是取得由系統自動產生的預設使用帳號及密碼，該資訊則是被存於 /etc/mysql/debian.cnf 檔案之中，我們查看該檔案來獲得相關資訊。
```gherkin=
sudo vim /etc/mysql/debian.cnf
```
![](https://i.imgur.com/2oC2Loc.png)

3. 登入MySQL資料庫並開啟遠端登入權限
> 如果你所架設的資料庫未來會有從其他電腦（遠端）登入的需求，那從設定中開啟相關權限便是必要的，為了修改相關的設定，首先我們必須修改 /etc/mysql/mysql.conf.d/mysqld.cnf 這個檔案：
> 因為我的database要與line連接，所以會先把 bind-address 這行設為廣播位址(這樣也可以在遠端登入)
```gherkin=
bind-address = 0.0.0.0
```
> 曾經試過只設定到上一步，卻發現還是無法遠端登入，問題就出在，要將39行的**skip-external-locking**註解掉，此行應該是表示封鎖外部位址連結，註解掉就可以允許外部連結
```gherkin=
#skip-external-locking
```
4. 登入
```gherkin=
mysql -u root -p
#輸入密碼
```
> Reference : https://medium.com/@richiechao95/ubuntu-18-04-mysql-%E8%B3%87%E6%96%99%E5%BA%AB%E6%9E%B6%E8%A8%AD%E6%95%99%E5%AD%B8-c7139819576f


## Some Tips

1. 突然無法啟動可能是要重啟
```gherkin=
sudo serviece mysql start
```

## MySQL語法
1. 允許遠端存取 : 
```gherkin=
mysql> GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;
-- enable new configuration
mysql> flush privileges;
-- exit database
mysql> exit
```
###### tags: `Templates` `Documentation`
