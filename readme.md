
# 本项目基于 Spring Boot 2.1.5 使用sharding-sphere + Mybatis-Plus 实现分库分表

## 创建数据库ds0
CREATE DATABASE IF NOT EXISTS `ds0` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci */; 
USE `ds0`; SET NAMES utf8mb4; SET FOREIGN_KEY_CHECKS = 0; 
-- ---------------------------- 
-- Table structure for user_0 -- 
---------------------------- 
DROP TABLE IF EXISTS `user_0`; 
CREATE TABLE `user_0` ( `id` int(11) NOT NULL, `name` varchar(255) DEFAULT NULL, `age` int(11) DEFAULT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4; 
-- ---------------------------- 
-- Table structure for user_1 -- 
---------------------------- 
DROP TABLE IF EXISTS `user_1`; 
CREATE TABLE `user_1` ( `id` int(11) NOT NULL, `name` varchar(255) DEFAULT NULL, `age` int(11) DEFAULT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4; 
SET FOREIGN_KEY_CHECKS = 1;

## 创建数据库ds1
CREATE DATABASE IF NOT EXISTS `ds1` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci */; 
USE `ds1`; SET NAMES utf8mb4; SET FOREIGN_KEY_CHECKS = 0; 
-- ---------------------------- 
-- Table structure for user_0 -- 
---------------------------- 
DROP TABLE IF EXISTS `user_0`; 
CREATE TABLE `user_0` ( `id` int(11) NOT NULL, `name` varchar(255) DEFAULT NULL, `age` int(11) DEFAULT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4; 
-- ---------------------------- 
-- Table structure for user_1 -- 
---------------------------- 
DROP TABLE IF EXISTS `user_1`; 
CREATE TABLE `user_1` ( `id` int(11) NOT NULL, `name` varchar(255) DEFAULT NULL, `age` int(11) DEFAULT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4; 
SET FOREIGN_KEY_CHECKS = 1;

## 分库策略：根据id,偶数保存到数据库ds0,奇数保存到数据库ds01
ds$->{id % 2}

## 分片策略：
根据age,偶数保存到表user_0,奇数保存到user_1表
user_$->{age % 2}表示user表根据id模2，而分成2张表，表名称为user_0到user_1

## 根据id,偶数保存到表user_0,奇数保存到user_1表
user$->{id % 2} 表示user表根据id模2，而分成2张表，表名称为user0到user_1

## 添加测试数据
http://localhost:8080/insert?id=1&name=lhd&age=12    
http://localhost:8080/insert?id=2&name=lhd&age=13    
http://localhost:8080/insert?id=3&name=lhd&age=14    
http://localhost:8080/insert?id=4&name=lhd&age=15

## 查询测试数据
http://localhost:8080/select