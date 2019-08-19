## PHP的运行机制
```
PHP的内部运行可以分为三个模块
(1):PHP内核  
	用来处理请求，文件流，错误处理等相关操作
(2):Zend引擎
   （ZE）将源文件转换成为机器语言，然后在虚拟机上面运行
(3):扩展层
    是一组函数,类库，流，PHP用他们来进行一些特定的操作
(4): SAPI  
    使得PHP本身与应用上层进行解耦，从而实现PHP与外部的交互    
    

最后，ZE将运行结果返回到PHP内核，再讲结果传送到SAPI层，
最后输出到浏览器上面
```

## PHP的启动流程
```
1.CLI运行模式
PHP_module_startup(模块初始化)   
php_request_start（请求初始化阶段）
php_execute_script(脚本执行阶段)
php_request_shutdown（请求关闭阶段）
php_module_shutdown(模块关闭阶段)


2.web运行模式
PHP_module_startup(模块初始化)

##常驻内存开始
fast_accept_request
php_request_startup
fpm_request_exexution
php_execute_script(脚本执行阶段)
fpm_request-end
php_request-shutdown
## 常驻内存结束

php_module_shutdown(模块关闭阶段)


php_execute_script:在这个模块中，大概过程有三步
(1)先读取PHP的代码，进行词法解析(Lexing)成token;
(2)语法解析，将token解析成抽象语法树
(3)将表达式编译成Opcode


关于Opcache
opcache 是一层加速机制，因为php是解释性语言中间会生成opcode交由zend引擎执行。


minit 模块初始化，rinit请求初始化，
对应的分别是结束时候触发mshutdown，rshutdown， 
顺序 minit-rinit-rshutdown-mshutdown  


```


## SAPI
```

```

## PHP垃圾回收机制
```
```

## 引用计数
```
引用计数在内存回收、字符串操作等地方使用非常广泛。

Zval的引用计数通过成员变量is_ref和ref_count实现，通过引用计数，多个变量可以共享同一份数据。避免频繁拷贝带来的大量消耗。

在进行赋值操作时，zend将变量指向相同的zval同时ref_count++，在unset操作时，对应的ref_count-1。

只有ref_count减为0时才会真正执行销毁操作。如果是引用赋值，则zend会修改is_ref为1
```

## 写时拷贝
```
PHP变量通过引用计数实现变量共享数据，那如果改变其中一个变量值呢？

当试图写入一个变量时，Zend若发现该变量指向的zval被多个变量共享，

则为其复制一份ref_count为1的zval，并递减原zval的refcount，这个过程称为“zval分离”。

可见，只有在有写操作发生时 zend才进行拷贝操作，因此也叫copy-on-write(写时拷贝)
```











