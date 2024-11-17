修改原因
当filetype为orc时，原hdfsreader在读取时会按照orc文件schema来组成列信息，而在某些场景下表中文件是通过文件导入方式入库，会导致实际列与orc schema 不匹配
例如
desc table: 
id string,name string,age int
orc file :
age int,id string,name string
datax json:
index 0 string,index 1 string,index 2 int
record reader:
age->string , id -> string , name -> int
导致读取时会出现会匹配错误类型

修改内容
修改DFSUtil.java 生成record 逻辑 使其从datax json中获取列信息
