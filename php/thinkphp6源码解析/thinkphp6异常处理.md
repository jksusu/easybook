#### tp6.0异常处理

##### tp6.0在框架初始化的时候 注册了四个函数
```php
    //Error.php   vendor/topthink/framework/src/think/initializer/Error.php
    public function init(App $app)
    {
        $this->app = $app;
        error_reporting(E_ALL);//设置应该报告何种 PHP 错误 E_ALL 为所有错误。
        set_error_handler([$this, 'appError']);//设置用户自定义的错误处理函数
        set_exception_handler([$this, 'appException']);//设置用户自定义的异常处理函数
        register_shutdown_function([$this, 'appShutdown']);//注册一个会在php中止时执行的函数
    }
```
* ErrorException.php  vendor/topthink/framework/src/think/exception/ErrorException.php
* Handle.php          vendor/topthink/framework/src/think/exception/Handle.php
> 在程序错误和php中止执行时 都会将错误传递给 ErrorException 类处理。在程序出异常的时候 最终处理的类是 Handle类
Handle类 通过 App获取到了程序的一些设置和配置文件，针对性的输出，记录异常日历。


##### 关于错误和异常
> 个人理解错会导致php脚本停止运行，而异常会抛出。在tp6.0中，错误级别导致脚本停止运行或者 异常处理导致脚本抛出异常都会触发
appShutdown 方法，由 ErrorException 类处理。实际上还是更具 Throwable 接口定义的那些方法来的。
在业务中，根具自己业务形态继承 Exception 类来统一进行处理。

```php
//对系统所有未查询到结果进行统一异常封装返回给前端
class SelectNoDataException extends \Exception{};//为查询到结果的异常类

//统一进行异常抛出
class ExceptionHandle extends Exception
{
      //伪代码
      public function render(Throwable $e)
      {
             //统一对未查询到结果的异常处理返回给前端
             if ($e instanceof SelectNoDataException) {
                 // ''''' 其他操作
                 return json_encode(['code'=>10000,'message'=>'未查询到结果']);
             }
      }
}
```
* php7错误处理 https://www.php.net/manual/zh/language.errors.php7.php




