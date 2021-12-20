# Redis

## 数据类型

### 通用命令

1. DEL del key 删除指定key

### **list**

#### list 是基于LinkedList实现的

1. LPUSH/RPUSH *lpush key element ...*  压入数据到列表左侧/右侧  
2. LPOP/RPOP   `lpop  key`              从列表删除最左边/右边的元素  
3. BRPOP/BLPOP `brpop key timeout`      阻塞操作，从列表中取出数据,如果没有数据则等待timeout时间,返回值为key 加上对应的元素  
4. LTRIM       `ltrim key start end`    截取功能，截取指定范围的list，范围之外的丢弃  
5. LRANGE `lrange key start end` 返回范围内的数据，但是不移除范围之外的数据
    > start 和 end 都可以为负数，负数表示从右边向左的下标  

### **Hash**  

1. HMSET `hmset key property property_value` 类似对象的存储方式  
2. HGET `hget key property` 获取hash对象的属性,只能获取单个
3. HMGET `hmget key property0 property1`同时获取多个属性  
4. HINCRBY `hincrby key property value`单独操作一个属性增加value  
5. HGETALL `hgetall key`获取所有属性

### **Set**

> set 是无序集合

1. SADD `sadd key value...`向set中添加数据  
2. SMEMBERS `smembers key` 返回Set中所有的数据  
3. SISMEMBER `sismember key value`测试set中是否包含value，返回1表示有存在，0不存在  
4. SINTER `sinter key key key`获取几个set的交集  
5. SPOP `spop key`从set中移除一个元素，移除并不是按照放入的顺序移除
6. SUNIONSTORE `sunionstore dest key key`取集合并集并放入dest的set
7. SCARD `scard key`统计数量  
8. SRANDMEMBER `srandmember key count`随机选择set中的count个元素,但是不删除元素

### **Redis Sorted Set**

> sorted set 有序集合  

1. ZADD `zadd key score value ...`添加数据
2. ZRANGE `zrange key start end [withscores]`获取集合数据,如果有参数会返回集合中的数据的score  

   ```bash
    127.0.0.1:6379> zrevrange sort_set 0 -1 withscores
      1) "Alast"
      2) "1960"
      3) "bsdad"
      4) "1950"
      5) "astrs"
      6) "1940"
   ```

   > Note: 0 and -1 means from element index 0 to the last element (-1 works here just as it does in the case of the LRANGE command)  
3. ZREVRANGE `zrevrange key start end`逆向获取数据  
4. ZRANGEBYSCORE `zrangebyscore key scoreStart scoreEnd`  

   ```bash
    127.0.0.1:6379> zrangebyscore sort_set -inf 1950
    1) "astrs"
    2) "bsdad"
   ```

   > -inf 表示负无穷大  

5. ZREMRANGEBYSCORE `zremrangebyscore key scoreStart scoreEnd` 移除指定范围的元素,返回移除元素的个数  
6. ZRANK `zrank key setValue` 返回setvalue在集合中的索引
7. ZREVRANK `zrevrank key value` 返回value在集合中的逆向索引
8  ZRANGEBYLEX `zrangebylex key min max`返回字典排序

### **Bitmap**

> bitmap 本身不是一种数据类型，只是一种对`string`类型的二进制操作，因为`string`类型的最大长度可以达到512MB,所以可以设置2的32次方的二进制位

1. SETBIT `setbit key offset value` 设置key的offset位的bit值
2. GETBIT `getbit key offset` 获取key的第offset位bit值
3. BITOP AND destkey srckey1 srckey2 srckey3 ... srckeyN
4. BITOP OR destkey srckey1 srckey2 srckey3 ... srckeyN
5. BITOP XOR destkey srckey1 srckey2 srckey3 ... srckeyN
6. BITOP NOT destkey srckey