---
title: Java 工具类
date: 2018-06-13 14:16:55
categories:
	- Java
tags:
	- Random
	- Date
	- jackson
description: "常用的工具类"
copyright: true
---

# 随机数

```
public static String getStringRandom(int length) {
    StringBuffer sb = new StringBuffer();

    Random random = new Random();

    //参数length，表示生成几位随机数
    for (int i = 0; i < length; i++) {

        String charOrNum = random.nextInt(2) % 2 == 0 ? "char" : "num";
        //输出字母还是数字
        if ("char".equalsIgnoreCase(charOrNum)) {
            //输出是大写字母还是小写字母
            int temp = random.nextInt(2) % 2 == 0 ? 65 : 97;
            char c = (char) (random.nextInt(26) + temp);
            sb.append(c);
        } else if ("num".equalsIgnoreCase(charOrNum)) {
            String s = String.valueOf(random.nextInt(10));
            sb.append(s);
        }
    }
    return sb.toString();
}
```


# 日期时间类

当前毫秒数 `System.currentTimeMillis()`

### 格式化时间

```
Date date = new Date();
SimpleDateFormat sdFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String str = sdFormatter.format(date);
System.out.println(str);
```
### 6小时后的时间

```
Date date = new Date();
Calendar calendar = Calendar. getInstance();
calendar.setTime(date);
// 6 小时后
calendar.add(Calendar.HOUR_OF_DAY, 6);

// 1 天后的 0:0:0
calendar.set(Calendar. HOUR_OF_DAY, 0);
calendar.set(Calendar. MINUTE, 0);
calendar.set(Calendar. SECOND, 0);
calendar.set(Calendar. MILLISECOND, 0);
calendar.add(Calendar. DAY_OF_MONTH, 1);

// 转为 date
date = calendar.getTime();
```

### 日期转时间戳

```
System.out.println(Calendar.getInstance().getTimeInMillis());
System.out.println(new Date().getTime());
```

### 时间戳转日期

```
Date date = new Date(System.currentTimeMillis());
```

# jackson

Jackson提供了一系列注解，方便对JSON序列化和反序列化进行控制

+ @JsonIgnore 此注解用于属性上，作用是进行JSON操作时忽略该属性。
+ @JsonFormat 此注解用于属性上，作用是把Date类型直接转化为想要的格式，如@JsonFormat(pattern = "yyyy-MM-dd HH-mm-ss")。
+ @JsonProperty 此注解用于属性上，作用是把该属性的名称序列化为另外一个名称，如把trueName属性序列化为name，@JsonProperty("name")。

### 对象转 JSON

ObjectMapper是JSON操作的核心，Jackson的所有JSON操作都是在ObjectMapper中实现。  
ObjectMapper有多个JSON序列化的方法，可以把JSON字符串保存File、OutputStream等不同的介质中。

+ writeValue(File arg0, Object arg1)把arg1转成json序列，并保存到arg0文件中。
+ writeValue(OutputStream arg0, Object arg1)把arg1转成json序列，并保存到arg0输出流中。
+ writeValueAsBytes(Object arg0)把arg0转成json序列，并把结果输出成字节数组。
+ writeValueAsString(Object arg0)把arg0转成json序列，并把结果输出成字符串。


```
ObjectMapper mapper = new ObjectMapper();

//User类转JSON
String json = mapper.writeValueAsString(user);
System.out.println(json);

//Java集合转JSON
List<User> users = new ArrayList<User>();
users.add(user);
String jsonlist = mapper.writeValueAsString(users);
System.out.println(jsonlist);
```

### JSON 转对象

```
String json = ...
// ObjectMapper支持从byte[]、File、InputStream、字符串等数据的JSON反序列化。
ObjectMapper mapper = new ObjectMapper();
User user = mapper.readValue(json, User.class);
System.out.println(user);
```

# 鸣谢

+ https://www.cnblogs.com/winner-0715/p/6109225.html