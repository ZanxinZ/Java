## Date

### 在当前时间基础上，加上 10s 偏移

```java
Calendar calendar = Calendar.getInstance();
calendar.add(Calendar.SECOND, 10);
Date date = calendar.getTime();
```

