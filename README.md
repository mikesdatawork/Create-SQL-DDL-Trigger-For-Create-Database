![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Create SQL DDL Trigger For Create Database
**Post Date: August 30, 2016**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process


![Create SQL DDL Trigger For Auditing]( https://mikesdatawork.files.wordpress.com/2015/11/process-automation-on-the-metal-gears-l.jpg?w=829 "Create SQL DDL Trigger") 
 
<p>Process Automation For Database Management.
Here's some quick SQL logic to create a DDL Trigger which is fired whenever a new database is created. This isn't too terribly hard to setup, but doesn't hurt to put a post out there for you guys. This is incredibly useful whenever you're setting up a new SQL Server environment, and you want to make sure that all the databases are configured within a uniform standard.
In this case… I have a trigger executing a Job simply called 'SET DATABASE CONFIGS', and within this job there is some SQL logic that sets recovery models etc. To take it a step further; my job will also notify the database group whenever new databases are added to this server environment.
Here's the Trigger logic:</p>



## SQL-Logic
```SQL
use master;
set nocount on
if exists(select 1 from sys.server_triggers where name = 'ddl_trig_run_job_on_create_database')
drop trigger ddl_trig_run_job_on_create_database on all server
go
 
create trigger ddl_trig_run_job_on_create_database on all server for create_database as
declare @new_database varchar(255)
set @new_database = cast(eventdata().query('/event_instance/databasename[1]/text()') as NVarchar(128))
exec msdb..sp_start_job @job_name = 'SET DATABASE CONFIGS';
go
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

