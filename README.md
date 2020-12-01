---
title: 'Project documentation template'
disqus: hackmd
---

Database 建置過程中遇到的問題
===


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


## Appendix and FAQ

:::info
**Find this document incomplete?** Leave a comment!
:::

###### tags: `Templates` `Documentation`
