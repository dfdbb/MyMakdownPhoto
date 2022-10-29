1.添加相应依赖
2.修改配置文件
3.注入redistemple类
4.调用redistemple.xxxx


```java
//redis测试  
@PostMapping("/set")  
public void set(@RequestBody User user){  
    redisTemplate.opsForValue().set("user",user);  
}  
//redis取测试  
@GetMapping("/get/{key}")  
public User get(@PathVariable("key") String key){  
    return (User) redisTemplate.opsForValue().get(key);  
}  
  
//redis删除测试  
@DeleteMapping("/delete/{key}")  
public boolean redisdelete(@PathVariable("key") String key){  
    redisTemplate.delete(key);  
 return redisTemplate.hasKey(key);  
}
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA2MTAzNzIzNiwtMTMzODk0NDAyNl19
-->