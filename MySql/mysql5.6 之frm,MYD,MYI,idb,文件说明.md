# mysql5.6 之frm,MYD,MYI,idb,文件说明

如数据库a，数据库表b
**如果表格b采用MyISAM**，data\a中会产生3个文件：
1. b.frm ：描述表结构文件，字段长度等
2. b.MYD(MYData)：数据信息文件，存储数据信息(如果采用独立表存储模式)
3. b.MYI(MYIndex)：索引信息文件。

**如果表格b采用InnoDB**，data\a中会产生1个或者2个文件：
1. b.frm ：描述表结构文件，字段长度等
2. 如果采用独立表存储模式，data\a中还会产生b.ibd文件（存储数据信息和索引信息）