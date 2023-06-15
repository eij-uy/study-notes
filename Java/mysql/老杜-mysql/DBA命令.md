[toc]

# DBA 命令

## 新建用户

`create user 用户名 identified by 密码`

## 授权

## 回收权限

## 导入导出: 数据的备份 ( 重点掌握 )

### 导出

注意：**在 windows 的 dos 命令窗口中**

#### 全部导出

`mysqldump 数据库名称>路径 + sql文件名`

#### 导出指定表

`mysqldump 数据库名称 表名称>路径 + sql文件名`

### 导入

注意：**在 mysql 下**

`source 导出来的 sql 文件`