## 异常捕获

```java
try {
    ...
    }catch(Exception e){
	...
    }finally{
     ...
    }
```



### finally 中的内容早于 return 的内容执行

```java
public class Run {
    public static void main(String[] args) {
        Run run = new Run();
        System.out.println(run.method());
    }
    private int method(){
        try {
            Thread.sleep(1000);
            return 5;
        }catch (InterruptedException e){

        }finally {
            System.out.println("可");
        }

        return 4;
    }
}
```

运行结果

> 可
> 5



***

