redis  key 过期时间

redis key过期时间实现有主动和被动两种

被动 是key 在读取的时候检测过期时间

主动定时轮询过期key 

**现在想在redis强制key 设置ttl**

redis命令类型中只有 string 类型有expire参数，其他类型 list、 set、sorted set、hash如果要设置过期时间要调用expire命令


----------


    修改db.c 78行后加下面代码
    //add by laoli 检测是否设置了过期时间
    if (o) {
    	long long expire = getExpire(c->db, key);
    	robj *setExpireTip = createObject(REDIS_STRING,sdsnew(
    	        "-ERR must set key expire!\r\n"));
    	if (expire < 0) {
    		addReply(c, setExpireTip);
    		return NULL;
    	}
    }
    


----------
> 127.0.0.1:6379> set name lijianwei
OK

127.0.0.1:6379> get name
(error) ERR must set key expire!





    
    

