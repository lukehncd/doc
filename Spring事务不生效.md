在Spring boot中启用事务很简单：
  1.  在入口添加@EnableTransactionManagement。
  2.  然后我们一般在服务层添加@Transactional,表示该方法启用事务管理。

但是有些时候，事务却不生效，可能出现的情况如下：
  1. 如果在Dao层的某个方法启用了事务管理，而该方法又在调用的Service层启用，且Service也启用了事务管理，则会报错说Dao方法无法被注入。
      -- 一般来说Dao层不需要事务管理，我们应尽可能的把Dao里面的复杂操作简化分解。
  2. 默认情况下aop只捕获 RuntimeException 的异常，但可以通过配置来捕获特定的异常并回滚。
     换言之，我们在Service层定义了一组操作，只要catch中没有捕获到RuntimeException，则默认情况下，事务是不会回滚的。
      -- 解决办法一：在catch中，抛出RuntimeException异常：return new RuntimeException(),然后在上一层进行事务管理。
         解决办法二：在catch中，添加手动回滚代码：TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
