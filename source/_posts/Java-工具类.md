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

## 格式化时间

```
Date date = new Date();
SimpleDateFormat sdFormatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String str = sdFormatter.format(date);
System.out.println(str);
```

## 6小时后的时间

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

## 日期转时间戳

```
System.out.println(Calendar.getInstance().getTimeInMillis());
System.out.println(new Date().getTime());
```

## 时间戳转日期

```
Date date = new Date(System.currentTimeMillis());
```

# jackson

Jackson提供了一系列注解，方便对JSON序列化和反序列化进行控制

+ @JsonIgnore 此注解用于属性上，作用是进行JSON操作时忽略该属性。
+ @JsonFormat 此注解用于属性上，作用是把Date类型直接转化为想要的格式，如@JsonFormat(pattern = "yyyy-MM-dd HH-mm-ss")。
+ @JsonProperty 此注解用于属性上，作用是把该属性的名称序列化为另外一个名称，如把trueName属性序列化为name，@JsonProperty("name")。

## 对象转 JSON

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

## JSON 转对象

```
String json = ...
// ObjectMapper支持从byte[]、File、InputStream、字符串等数据的JSON反序列化。
ObjectMapper mapper = new ObjectMapper();
User user = mapper.readValue(json, User.class);
System.out.println(user);
```

# enum 枚举

## 枚举定义

```
public enum Color {
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
    // 成员变量
    private String name;
    private int index;

    // 构造方法
    private Color(String name, int index) {
        this.name = name;
        this.index = index;
    }

    // 普通方法
    public static String getName(int index) {
        for (Color c : Color.values()) {
            if (c.getIndex() == index) {
                return c.name;
            }
        }
        return null;
    }

    // get set 方法
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getIndex() {
        return index;
    }

    public void setIndex(int index) {
        this.index = index;
    }
}
```

## 枚举使用

```
Color c = Color.GREEN;
System.out.println(c.toString());
System.out.println(c.getIndex());
System.out.println(Color.getName(3));
c.setName("aa");
c.setIndex(33);
System.out.println(c.toString());
```

结果

```
super=GREEN i=2 n=绿色
2
白色
super=GREEN i=33 n=aa
```

# HTTP 请求

+ 使用原生 `HttpURLConnection`
+ 使用 `HTTPClient`


这里使用 `HttpURLConnection` 实现 `POST请求`, 如果是`GET请求`则去掉 `param` 和 `OutputStream` 即可

```
public static String doPost(String httpUrl, String param) {

    HttpURLConnection connection = null;
    InputStream is = null;
    OutputStream os = null;
    BufferedReader br = null;
    String result = null;
    try {
        URL url = new URL(httpUrl);
        // 通过远程url连接对象打开连接
        connection = (HttpURLConnection) url.openConnection();
        // 设置连接请求方式
        // 如果是`GET请求`则去掉 `param` 和 `OutputStream` 即可
        connection.setRequestMethod("POST");
        // 设置连接主机服务器超时时间：15000毫秒
        connection.setConnectTimeout(15000);
        // 设置读取主机服务器返回数据超时时间：60000毫秒
        connection.setReadTimeout(60000);

        // 默认值为：false，当向远程服务器传送数据/写数据时，需要设置为true
        connection.setDoOutput(true);
        // 默认值为：true，当前向远程服务读取数据时，设置为true，该参数可有可无
        connection.setDoInput(true);
        // 设置传入参数的格式:请求参数应该是 name1=value1&name2=value2 的形式。
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        // 设置鉴权信息：Authorization: Bearer da3efcbf-0845-4fe3-8aba-ee040be542c0
        connection.setRequestProperty("Authorization", "Bearer da3efcbf-0845-4fe3-8aba-ee040be542c0");
        // 通过连接对象获取一个输出流
        os = connection.getOutputStream();
        // 通过输出流对象将参数写出去/传输出去,它是通过字节数组写出的
        os.write(param.getBytes());
        // 通过连接对象获取一个输入流，向远程读取
        if (connection.getResponseCode() == 200) {

            is = connection.getInputStream();
            // 对输入流对象进行包装:charset根据工作项目组的要求来设置
            br = new BufferedReader(new InputStreamReader(is, "UTF-8"));

            StringBuffer sbf = new StringBuffer();
            String temp = null;
            // 循环遍历一行一行读取数据
            while ((temp = br.readLine()) != null) {
                sbf.append(temp);
                sbf.append("\r\n");
            }
            result = sbf.toString();
        }
    } catch (MalformedURLException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        // 关闭资源
        if (null != br) {
            try {
                br.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (null != os) {
            try {
                os.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (null != is) {
            try {
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        // 断开与远程地址url的连接
        connection.disconnect();
    }
    return result;
}
```

# IO

+ 读入 `InputStream.read`
+ 写出 `OutputStream.write`

## 文件下载 在线打开

```
File f = new File(filePath);
if (!f.exists()) {
    response.sendError(404, "File not found!");
    return;
}

// 非常重要
response.reset();

if (isOnLine) {
    // 在线打开方式
    URL u = new URL("file:///" + filePath);
    response.setContentType(u.openConnection().getContentType());
    response.setHeader("Content-Disposition", "inline; filename=" + f.getName());
    // 文件名应该编码成UTF-8
} else {
    // 纯下载方式
    response.setContentType("application/x-msdownload");
    response.setHeader("Content-Disposition", "attachment; filename=" + f.getName());
}

OutputStream out = response.getOutputStream();


BufferedInputStream br = new BufferedInputStream(new FileInputStream(f));
byte[] buf = new byte[1024];
int len = 0;
while ((len = br.read(buf)) > 0){
    out.write(buf, 0, len);
}
br.close();
out.close();
```


与 while 的对比
```
InputStream fis = new BufferedInputStream(new FileInputStream(path));
byte[] buffer = new byte[fis.available()];
fis.read(buffer);
fis.close();

OutputStream out = response.getOutputStream();
out.write(buffer);
out.flush();
out.close();

```

# 鸣谢

+ https://www.cnblogs.com/winner-0715/p/6109225.html
+ http https://www.cnblogs.com/hhhshct/p/8523697.html