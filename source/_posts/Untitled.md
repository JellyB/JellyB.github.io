title: Redis 使用手册
author: jelly
tags:
  - redis
categories:
  - redis
date: 2018-05-24 17:39:00
---
##### Redis 数据结构：字符串String

- `set` 设置一个字符串，并设置字符穿的值value

    ```
        set world helloworld
        OK
    ```

- `get` 获取一个字符串的value

    ```
        get world
        "helloworld"
    ```

- `srtlen` 获取字符穿的长度

    ```
        set hello helloworld
        get hello
        "helloworld"
        setlen hello
        (integer) 10
    ``` 
    
- `getrange` 获取字符串区间闭合[]区间

    ```
        set word helloworld
        getrange world 0 2
        "hel"
    ```
    
- `append` 追加到当前key的value的末尾，控制台输出当前k append操作之后的value的长度值。
    
    ```
        set 1 1
        OK
        append 1 hello
        (integer) 6
        get 1
        "1hello"
    ```

- `setex` 设置k的值value，并设置有效期，`ex`是expire的缩写，比如设置c的值为c，有效期为100s

    ```
        setex c 100 c
        ttl c
    ```
     
- `psetex` 单位为毫秒，比 setex 时间降了一个级别，设置d的value为d，有效期为10000毫秒。

    ```
        psetex d 10000 d
    ``` 

- `getset` 先get操作后set，在set一个值得时候，可以通过返回值获取到旧的值。

    ```
        set a a
        get a
        "a"
        getset a aaa
        "a"
        get a
        "aaa"
    ```
    
- `mset`和`mget`  mset可以设置多个k-v mset a1 a1 b1 b1 c1 c1，mget可以同时获取多个k的v

    ```
        mset a1 a1 b1 b1 c1 c1
        OK
        mget a1 b1 c1
        1) "a1"
        2) "b1"
        3) "c1"
    ```
    
- `setnx` 当且仅当当前的k不存在时才会设置v成功，否则，如果k存在，则设置不成功。nx = not exist 的缩写，此为setnx与set的区别。

    ```
        set a a
        get a
        "a"
        setnx a newvalue
        (integer) 0
        setnx newkey newvalue
        (integer) 1
        get newkey
        "newvalue"
    ```
    
- `msetnx`  setnx命令的拓展，批量操作多个k，msetnx拥有原子性，要么都成功，要么都失败，下面例子中，因为q存在，所以执行msetnx q q u u  时，因为q已经存在，u没有被设置成功。

    ```
        msetnx  q q w w 
        (integer) 1
        keys * 
        1) "q"
        2) "w"
        msetnx q q u u
        (integer ) 0
    ```
    
    
- `incr` 对数值类型的str的value进行数值 `+` 操作，必须为数值类型，否则会报错.可以自定义步长，使用`incrby`命令，后面加上步长。

    ```
        get 1
        (nil)
        set 1 1
        OK
        get 1
        "1"
        incr 1
        (integer) 2
        incr 1
        (integer) 3
        incrby 1 100
        (integer) 103
        incr a
        (error) ERR value is not an integer or out of range
    ```
    
- `decr` 对数值类型的str的value进行数值 `-` 操作，与`incr`操作相反。同样提供`decrby`操作。
    
    ```
        get 1 100
        "105"
        decr 1 
        (integer) 104
        decrby 1 100
        (integer) 4
        decrby 1 5
        (integer) -1
        decr a 
        (error) ERR value is not an integer or out of range
    ```
    
    
    ***
    
##### Redis数据结构:哈希hash

> hash 结构，不同于string操作，只设置k，v即可。 hash 除了需要设置k，v之外，还有一个`field`的概念。即 key+field 定位到一个value，而且一个key，可以有多个field。
 
- 操作hash数据结构之前，切换到另外一个工作空间。
 
    ```
        select 1
        OK
        key *
        (empty list or set)
    ```

- `hset` 设置一个key + field指定value
    
    ```
        hset map name jelly
        (integer) 1
        keys *
        "map"
        type map
        hash
    ```

- `hget` 指定key + field获取 hash 的value，如果不存在返回`nil`。
    
    ```
        hget map name
        "jelly"
        hget map age 
        (nil) 
    ```
    
- `hexists` 用来判断制定key和field的hash数据是否存在。
    
    ```
        hexists map name
        (integer) 1
        hexists map name123
        (integer) 0
    ```

- `hgetall` 获取当前key所有的filed和value。
    
    ```
        hset map name jelly
        keys * 
        "map"   
        hset map age 18
        key *
        "map"
        hgetall map
        1) "name"
        2) "jelly"
        3) "age"
        4) "18"
    ```
    
- `hkeys` 获取当前key的所有field。
    
    ```
        hkeys map
        1) "name"
        2) "age"
    ```

- `hvals` 获取当前key的所有value。
    
    ```
        hvals map
        "jelly"
        "18"
    ```
    
- `hlen` 获取当前key的长度，即field的数量。

    ```
        hlen map
        (integer) 2
    ```

- `hmset` 设置多个field的value。不要忘了指定 key
    
    ```
        hmset map key1 value1 key2 value2
        OK
        hlen map
        (integer) 4
    ```
    
- `hmget` 指定多个field获取多个对应的 value
    
    ```
        hmget map key1 key2 name age
        1) "value1"
        2) "value2"
        3) "jelly"
        4) "18"
        hlen map
        (integer) 4
    ```

- `hdel` 删除指定key下的 field和value
    
    ```
        hdel map key1 key2
        (integer) 2
        hkeys map
        1) "name"
        2) "age"
    ```

- `hsetnx`， 以nx结尾的命令会有一个判断是否存在的过程，如果不存在，设置成功，否则设置失败
    
    ```
        hsetnx map name jellyb
        (integer) 0
        hsetnx map address beijing
        (integer) 1
    ```

***

##### Redis数据结构：列表list

> 可以认为和我们java中list非常相似的一个列表，允许重复值存在

- 切换工作空间
    
    ```
        select 2
        keys *
        (empty list or set)
    ```
    
- `lpush` 生成一个list，后放的元素在前面
    
    ```
        lpush list 1 2 3 4 5 6 7 8 9 9 9 0
        (integer) 12
        keys *
        list
        type list
        list
        llen list
        (integer) 12
    ```
    
- `lindex` 获取指定index的value
    
    ```
        lindex list 0
        "12"
    ```
    
- `lrange` 获取list指定区间的元素，如果start等于value，效果等于lindex
    
    ```
        lrange list 0 2
        1) "0"
        2) "9"
        3) "9"
        lrange list 0 0 
        1) "0"
    ```
    
- `lset` 设置list指定index的value
    
    ```
        lset list 0 12
        OK
        lindex list 0
        1) "12"
   ```

- `lpop` 和 `rpop`  lpop移除list index =0位置的元素，rpop移除`index=llen list -1`位置的元素
    
    > 先修改list元素为0 - 11 从小到大排列
    
    ```
        lrange list 0 100
        1) "12"
        2) "9"
        3) "9"
        4) "8"
        ...
        12) "0"
        lset list 0 11
        OK
        lset list 1 10
        OK
        lset list 2 9
        OK
        lpop list
        "11"
        rpop list
        "0"
        lrange list 0 100
        1) "10"
        2) "9"
        ...
        10) "1"
        
***

Redis数据结构：集合set

> set 无序集合，可排除重复，set是使用hash实现的，查找删除元素等操作的时间复杂度都是O(1)

- 切换命名空间
    
    ```
        select 3
        keys * 
        (empty list or set)
    ```
    
- `sadd` 添加元素到指定key中，控制台返回每次成功添加到集合中的元素的个数，如果添加已经存在的元素，此元素不会被添加
    
    ```
        sadd set a b c d
        (integer) 4
        keys *
        "set"
        type set
        set
        sadd set a h
        (integer) 1
    ```
    
- `scard` 返回指定set集合的长度

    ```
        scard set
        (integer) 5
    ```
    
- `srename` 重命名set集合
    
    ```
        srename set set1
        OK
        keys *
        "set1"
    ```
    
- `smembers` 罗列set集合中的元素，无序展示
    
    ```
        smembers set1
        1）"d"
        2）"c"
        3）"b"
        4）"h"
        5）"a"
    ```

- `sdiff` 求两个集合的差集，A `-` B，比较两个set集合A与B，返回存在于A集合，并且不属于B集合的元素

    ```
        sadd set2 c d e f g h
        (integer) 6
        smembers set1
        1)"d"
        2)"c"
        3)"b"
        4)"h"
        5)"a"
        smembers set2
        1)"e"
        2)"g"
        3)"h"
        4)"d"
        5)"c"
        6)"f"
        sdiff set1 set2
        1)"a"
        2)"b"
        sdiff set2 set1
        1)"g"
        2)"e"
        3)"f"
    ```
    
- `sinter` 求两个集合的交集A `∩` B
> 以上述set1，set2元素为例这里就不使用`smembers` 作展示了
    
    ```
        keys *
        "set2"
        "set1"
        sinter set1 set2
        1)"h"
        2)"d"
        3)"c"   
    ```
    
- `sunion` 求两个结合的并集 A `∪` B 
> 同样以上述set1，set2元素为例
    
    ```
        sunion set1 set2
        1)"g"
        2)"e"
        3)"h"
        4)"a"
        5)"b"
        6)"d"
        7)"f"
        8)"c"
    ```
    
- `srandmember` 返回集合中随机`n`个元素
> 使用数据库中的set1集合作演示

    ```
        srandmember set1 2
        1)"a"
        2)"b"
        srandmember set1 3
        1)"h"
        2)"b"
        3)"c"
    ```
    
- `sismember` 判断给定元素是否属于当前集合
> 此处使用数据库中的set2集合作演示

    ```
        sismember set2 c
        (integer) 1
        sismember set2 a
        (integer) 0
    ```
    
- `srem` 移除指定集合的一个或多个元素，空格隔开
> 此处使用set2集合作演示
    
    ```
        srem set2 e f
        (integer) 2
        smembers set2
        1)"g"
        2)"h"
        3)"d"
        4)"c"
        srem set2 a
        (integer) 0
    ```

- `spop` 移除并返回一个随机元素
    
    ```
        keys *
        "set1"
        "set2"
        sadd set1 x y z
        (integer) 3
        smembers set1
        1)"y"
        2)"h"
        3)"a"
        4)"b"
        5)"d"
        6)"z"
        7)"x"
        8)"c"
        spop set1
        "c"
        spop set1
        "x"
        spop set1
        "d"
        smembers set1
        1)"y"
        2)"h"
        3)"a"
        4）"b"
        5）"z"
    ```
    
***

##### Redis数据结构：有序集合sortedset
> 有序集合sortedset也是使用hash实现的，因此它的添加查找删除操作的时间复杂度也是O(1)，它是使用`分数`来保证集合元素从小到大有序排序，其中分数是可以重复的，但是元素不可以重复。因此可以保证其中的元素不重复而且有序，比较像java中的 linkdhashset

- 切换命名空间
    
    ```
        select 4
        keys *
        (empty list or set)
    ```
    
- `zadd` 生成一个sortedset，并指定元素的分数
    
    ```
        zadd sortedset1 100 a 200 b 300 c 400 d
        (integer) 4
        type sortedset1
        zset
        rename sortedset1 sortedset
        OK
    ```

- `zcard` 查看sortedset集合中元素的数量
    
    ```
        zcard sortedset
        (integer) 4
    ```

- `zscore` 查看集合中指定元素的分数
    
    ```
        zscore sortedset a
        "100"
        zscore sortedset d
        "400"
    ```

- `zcount` 返回分数在指定闭合区间中元素的个数
    
    ```
        zcount sortedset 0 320
        (integer) 3
    ```

- `zrank` 返回元素在集合中的索引值
    
    ```
        zrank sortedset a
        (integer) 0
        zrank sortedset c
        (integer) 2
        zrank sortedset d
        (integer) 3
    ```
       
- `zincrby` 提高元素的分数，可以为`负数`
    
    ```
        zincrby sortedset 1000 a
        "1100"
        zscore sortedset a
        "1100"
        zrank sortedset a
        (integer) 3
        zrank sortedset b
        (integer) 0
        zincrby sortedset -1 b
        "199"
    ```
    
- `zrange` 返回索引在指定闭合区间中的元素，使用`withscores`附带返回元素的分数
    
    ```
        zrange sortedset 0 100
        1)"b"
        2)"c"
        3)"d"
        4)"a"
        zrange sortedset 0 2
        1)"b"
        2)"c"
        3)"d"
        zrange sortedset 0 3 withscores
        1)"b"
        2)"199"
        3)"c"
        4)"300"
        5)"d"
        6)"400"
        7)"a"
        8)"1100"
    ```

    
        
        





    
    

    

    
 

    
        
        
    
        
        
    
        
        
    

    
  


