springboot事务和异常处理：

```
在需要的方法上加注解@Transactional即可
```

注意：

1. 不需要在启动类上加@EnableTransactionManagement注解，开启事务，直接使用@Transactional即可
2. 事务只对RuntimeException异常起作用，其他异常spring认为是可容忍的，因此想要回滚就须使用throw new RuntimeException抛出异常。
3. RuntimeException不能用try—catch处理，否则事务不生效。因为try—catch已经处理了这个异常，spring就不会多管闲事再去处理。