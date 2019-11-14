![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 备份前将脱机数据库联机
### Bring Offline Databases Online Before Backup
**发布-日期: 2014年04月23日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
此脚本将在线设置所有脱机数据库，并在切换到联机后进行备份。我还添加了一些额外的逻辑来使用网络备份共享，并为文件名添加时间戳。

## English
This script will Set all Offline Databases Online, and do a backup after the switch to online. I’ve also taken the liberty of adding some extra logic to use a network backup share, and add a time stamp to the file name.

---
## Logic
```SQL
use master;
set nocount on
declare @timestamp 				varchar(50)
declare @servername 			varchar(50)
declare @backup_offlinedb 		varchar(max)
declare @set_databases_online 	varchar(max)
set 	@servername 			= ( select @@servername )
set 	@timestamp 				= ( select rtrim(replace(replace(replace(convert(char, getdate(), 9), ‘:’, ‘-‘), ‘AM’, ‘ am’), ‘PM’, ‘ pm’)) ) 
set 	@backup_offlinedb = ”
set 	@set_databases_online = ”
select @backup_offlinedb = @backup_offlinedb +
‘backup database [‘ + name + ‘] to disk = ”\\MyBackupShare\’ + @servername + ‘\’ + name + ‘\Full\’ + name + ‘ Full ‘ + @timestamp + ‘.bkp” with format; ‘ + char(10) from sys.databases where name like ‘EDDS%’ and state_desc = ‘offline’
select @set_databases_online = @set_databases_online +
‘alter database [‘ + name + ‘] set online; ‘ + char(10)
from sys.databases where /*name like ‘%wildcard here%’ and*/ state_desc = ‘offline’
exec (@set_databases_online);
waitfor delay ’00:00:30′ - wait 30 seconds for online operation to complete. you should be fine, but doesn’t hurt to add some delay. 

exec (@backup_offlinedb);
```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

