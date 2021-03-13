# Java8新特性

## 一、Java.time

### 1、Date-Time API中的基本类使用

Date-Time API中所有类均生成不可变实例，它们是线程安全的，并且这些类不提供公共构造函数，也就是说没办法通过new的方式直接创建，需要采用工厂方式加以实例化。

#### ①、now方法可以根据当前日期或时间创建实例对象

```java
//使用now方法创建Instant的实例对象
Instant instantNow = Instant.now();
//使用now方法创建LocalDate的实例对象
LocalDate localDateNow = LocalDate.now();
//使用now方法创建LocalTime的实例对象
LocalTime LocalTimeNow = LocalTime.now();
//使用now方法创建LocalDateTime的实例对象
LocalDateTime localDateTimeNow = LocalDateTime.now();
//使用now方法创建ZonedDateTime的实例对象
ZonedDateTime zonedDateTimeNow = ZonedDateTime.now();
```

```java
//将实例对象打印到控制台
System.out.println("Instant:"+instantNow);
System.out.println("LocalDate:"+localDateNow);
System.out.println("LocalTime:"+LocalTimeNow);
System.out.println("LocalDateTime:"+localDateTimeNow);
System.out.println("ZonedDateTime:"+zonedDateTimeNow);

Instant:2021-01-26T13:44:28.847Z
LocalDate:2021-01-26
LocalTime:21:44:28.854
LocalDateTime:2021-01-26T21:44:28.854
ZonedDateTime:2021-01-26T21:44:28.854+08:00[Asia/Shanghai]
```

```java
//使用now方法创建Year的实例对象
Year year = Year.now();
//使用now方法创建YearMonth的实例对象
YearMonth yearMonth = YearMonth.now();
//使用now方法创建MonthDay的实例对象
MonthDay monthDay = MonthDay.now();
```

```java
//打印控制台
System.out.println("Year:"+year);
System.out.println("YearMonth:"+yearMonth);
System.out.println("MonthDay:"+monthDay);

//打印控制台
System.out.println("Year:"+year);
System.out.println("YearMonth:"+yearMonth);
System.out.println("MonthDay:"+monthDay);
```

#### ②、of方法可以根据给定的参数生成对应的日期/时间对象

基本上每个基本类都有of方法用于生成对应的对象，而且重载形式多变，可以根据不同的参数生成对应的数据。(代码略)

**为LocalDateTime添加时区信息(拓展)**

之前的ZonedDateTime，这个对象里面封装的不仅有时间日期，并且还有偏移量+时区。

通过提供的一个类ZoneId的getAvailableZoneIds方法可以获取到一个Set集合，集合中封装了600个时区。

```java
//获取所有的时区信息
Set<String> availableZoneIds = ZoneId.getAvailableZoneIds();
for(String availableZoneId : availableZoneIds){
    System.out.println(availableZoneId);
}

//获取当前系统默认的时区信息 -> 中国
ZoneId zoneId = ZoneId.systemDefault();
System.out.println(zoneId);
```

```java
//1、封装LocalDateTime对象，参数自定义 -> 2018年11月11日 8点54分38秒
LocalDateTime localDateTime = LocalDateTime.of(2018, 11, 11, 8, 54, 38);
//2、localDateTime对象现在只是封装了一个时间，并没有时区相关的数据，所以要添加时区信息到对象中，使用atZone()方法
ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.of("Asia/Shanghai"));
System.out.println("Asia/Shanghai当前的时间是：" + zonedDateTime);
//3、更改时区查看其他时区的当前时间，通过withZoneSameInstant()方法即可更改
ZonedDateTime tokyoZonedDateTime = zonedDateTime.withZoneSameInstant(ZoneId.of("Asia/Tokyo"));
System.out.println("在同一时刻，Asia/Tokyo的时间是：" + tokyoZonedDateTime);

Asia/Shanghai当前的时间是：2018-11-11T08:54:38+08:00[Asia/Shanghai]
在同一时刻，Asia/Tokyo的时间是：2018-11-11T09:54:38+09:00[Asia/Tokyo]
```

#### ③、Month枚举类的作用

Java.time包中引入了Month的枚举，Month中包含标准日历中的12个月份的常量(从January到December)，也提供了一些方便的方法供我们使用。

推荐在初始化LocalDate和LocalDateTime对象的时候，月份的参数使用枚举的方式传入，这样更简单易懂而且不易出错。

```java
//在初始化LocalDate和LocalDateTime的时候，月份的参数传入枚举类Month -> 2011年5月15日11时11分11秒
LocalDateTime.of(2011, Month.MAY, 15, 11, 11, 11);
//Month枚举类 -> of可以根据传入的数字返回对应的月份枚举
Month month = Month.of(12);
System.out.println(month);

DECEMBER
```

#### ④、plus方法单独使用方式(扩展)

```java
//封装日期 -> 表示结婚的时间点
LocalDateTime marryTime = LocalDateTime.of(2020, Month.FEBRUARY, 2, 11, 11, 11);
//使用plus方法进行计算，加1个10年
LocalDateTime time = marryTime.plus(1, ChronoUnit.DECADES);
System.out.println("如果在" + marryTime + "结婚，那么锡婚的时间是：" + time);

如果在2020-02-02T11:11:11结婚，那么锡婚的时间是：2030-02-02T11:11:11
```

#### ⑤、with方法使用

```java
LocalDateTime time = LocalDateTime.now();
//经过使用之后发现time中的时间有错误，日期应该是1号 -> 在不知道原有时间的基础上，无法进行增减操作，所以可以直接使用with方法进行修改
LocalDateTime resultTime = time.withDayOfMonth(1);
System.out.println("修改前错误时间：" + time);
System.out.println("修改后正确时间：" + resultTime);

修改前错误时间：2021-01-31T11:38:13.266
修改后正确时间：2021-01-01T11:38:13.266
```

```java
LocalDateTime time = LocalDateTime.now();
//经过使用之后发现time中的时间有错误，日期应该是1号 -> 在不知道原有时间的基础上，无法进行增减操作，所以可以直接使用with方法进行修改
LocalDateTime resultTime = time.with(ChronoField.DAY_OF_MONTH, 1);
System.out.println("修改前错误时间：" + time);
System.out.println("修改后正确时间：" + resultTime);

修改前错误时间：2021-01-31T11:45:00.965
修改后正确时间：2021-01-01T11:45:00.965
```