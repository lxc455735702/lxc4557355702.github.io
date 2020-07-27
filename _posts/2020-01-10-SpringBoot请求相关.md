---
layout: post
title: "SpringBoot请求相关"
date: 2020-01-10 
description: "SpringBoot请求相关"
tag: Springboot
---

# 前言
 刚开始接触SpringBoot，查阅了一些关于接口的文章,如果有写的不对的地方，欢迎批评指正~
 
 # 注解
### 1.@RestController
 @RestController出现的频率很高，那它到底是什么意思呢？
 
 Spring中 @RestController 的作用等同于@Controller + @ResponseBody 所以要理解@RestController注解还得了解@Controller 和 @ResponseBody的注解
 
@Controller
@Controller是一种特殊化的@Component类，在实际操作中@Controller用来表示Spring某个类是否可以接受HTTP请求，它通常与@ResponseBody绑定使用。

@Component  
1.把普通POJO（Plain Ordinary Java Object简单的java对象）实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>  

2.泛指组件，当组件不好归类的时候，可以使用@Component注解进行标注

@ResponseBody
在实际操作中我们只需要在Controller层使用@RequestBody注解就可以将对象进行反序列化；而若需要对Controller的方法进行序列化，我们需要在返回值上使用@ResponseBody；也可以将@ResponseBody注解在Controller类上，这样可以将这个类中所有的方法序列化。

总结：
1.@RestController为开发提供了方便，在提供json接口时需要的配置操作再也不需要自己配置了
2.@RestController注解相当于@ResponseBody和@Controller的结合
3.@RestController注解时，返回的是内容实例

### 2.@RequestMapping
@RequestMapping是一个用来处理请求地址映射的注解，可用于类或者方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径

@GetMapping是一个组合注解，是@RequestMapping(method = RequestMethod.GET)的缩写。

@PostMapping是一个组合注解，是@RequestMapping(method = RequestMethod.POST)的缩写。

# 请求相关

这里尝试写一个请求

```
@RestController
public class UserController {

    @RequestMapping("/getUserInfo")
    public Map<String, Object> getUserInfo() {
        Map<String, Object> map = new HashMap<>();
        map.put("account", "123");
        map.put("name", "张三");
        return map;
    }
}
```
![请求.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTAwMDVjMmZjYTc1YTIwMDYucG5n?x-oss-process=image/format,png)


备注:  
1.ip地址是本机IPv4 地址  
2.RequestMapping 这里不管是post请求还是get 请求都是一样的返回值

### post请求

定义一个Person实体类

```
public class Person {
    private long id;
    private String name;
    private int age;
    private String hobby;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getHobby() {
        return hobby;
    }

    public void setHobby(String hobby) {
        this.hobby = hobby;
    }

    @Override
    public String toString() {
        return "name:" + name + ";age=" + age + ";hobby:" + hobby;
    }
}
```

在之前的UserController类里面添加如下方法：
```
@PostMapping(path = "/setUserInfo")
    public String setUserInfo(Person person) {
        System.out.println(person.toString());
        return person.toString();
    }
```
效果如下：
![post请求.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTYwY2Q0MzZkNmIyODhhZDIucG5n?x-oss-process=image/format,png)
备注：
但是发现 postMan 采用 raw 方式
data如下：  
{
 "name":"lxc",
 "age":13,
 "hobby":"test"
}

返回的结果：name:null;age=0;hobby:null

### 如何接受json数据
新写一个接口如下：
```
  @PostMapping(path = "/setUserInfo2")
  public String setUserInfo2(@RequestBody Person person) {
    return person.toString();
}
```
![postJson.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTc5MjgxM2Q0YjViMjRhZGMucG5n?x-oss-process=image/format,png)

备注：需要修改请求头的content-Type 为application/json

###  上传文件
这里直接贴代码
```
package com.example.demo;

import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.*;

@RestController
public class FileUploadController {

    @PostMapping("/singleFile")
    @ResponseBody
    public String singleFile(@RequestParam("file") MultipartFile file) {
        //判断非空
        if (file.isEmpty()) {
            return "上传的文件不能为空";
        }
        try {
            String path = "D:\\Program Files\\upload\\";
//            Log.info(this.getClass().getName() + "图片路径：" + path);
            File f = new File(path);
            //如果不存在该路径就创建
            if (!f.exists()) {
                f.mkdir();
            }
            File dir = new File(path + file.getOriginalFilename());
            // 文件写入
            file.transferTo(dir);
            return "上传单个文件成功";
        } catch (Exception e) {
            e.printStackTrace();
            return "上传单个文件失败";
        }
    }

    //文件下载相关代码
    @RequestMapping("/download")
    public String downloadFile(HttpServletRequest request, HttpServletResponse response) {
        String fileName = "test.txt";// 设置文件名，根据业务需要替换成要下载的文件名
        if (fileName != null) {
            //设置文件路径
            String realPath = "D://Program Files//upload";
            File file = new File(realPath, fileName);
            if (file.exists()) {
                response.setContentType("application/force-download");// 设置强制下载不打开
                response.addHeader("Content-Disposition", "attachment;fileName=" + fileName);// 设置文件名
                byte[] buffer = new byte[1024];
                FileInputStream fis = null;
                BufferedInputStream bis = null;
                try {
                    fis = new FileInputStream(file);
                    bis = new BufferedInputStream(fis);
                    OutputStream os = response.getOutputStream();
                    int i = bis.read(buffer);
                    while (i != -1) {
                        os.write(buffer, 0, i);
                        i = bis.read(buffer);
                    }
                    System.out.println("success");
                } catch (Exception e) {
                    e.printStackTrace();
                } finally {
                    if (bis != null) {
                        try {
                            bis.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                    if (fis != null) {
                        try {
                            fis.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }
        }
        return null;
    }
}
```
![上传结果.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTRhMWY0YzExNTJmYzg1ZjUucG5n?x-oss-process=image/format,png)
![下载结果.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTI4YmJhYWUxYmNiOGJiMWUucG5n?x-oss-process=image/format,png)

这里只是简单的举例子，当然还有多文件上传等，这个可以之后去慢慢摸索...emm..

# 参数校验 
### 统一的返回结果

有时候需要统一处理返回结果，这里简单写个例子

```
package com.example.demo.result;


import java.io.Serializable;

/**
 * 统一的返回结果集
 */
public class JsonResult implements Serializable {

    // 成功
    public static final int SUCCESS = 0;
    // 失败
    public static final int FAIL = 1;

    private int status;
    private int code;
    private String msg;
    private Object data;

    public JsonResult(int status, int code, String msg, Object data) {
        this.status = status;
        this.code = code;
        this.msg = msg;
        this.data = data;
    }

    public int getStatus() {
        return status;
    }

    public void setStatus(int status) {
        this.status = status;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }

    public static JsonResult success(Object data) {
        return new JsonResult(SUCCESS, 200, "ok", data);
    }

    public static JsonResult success(String msg) {
        return new JsonResult(SUCCESS, 200, msg, null);
    }

    public static JsonResult fail(String msg) {
        return fail(400, msg);
    }

    public static JsonResult fail(int code, String msg) {
        return new JsonResult(FAIL, code, msg, null);
    }

}

```

### 创建测试用户类


```
package com.example.demo.user;

import org.hibernate.validator.constraints.Length;

import javax.validation.constraints.NotNull;
import java.io.Serializable;

public class User implements Serializable {

    @Length(min = 5, max = 10, message = "用户名长度不合法")
    @NotNull(message = "用户名不能为空")
    private String username;

    @Length(min = 6, max = 16, message = "密码长度不合法")
    @NotNull(message = "密码不能为空")
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
### UserController新增测试接口

```
    @PostMapping("/login")
    public JsonResult login(@Validated @RequestBody User user) {
        return JsonResult.success("登陆成功");
    }

    //测试get请求
    @GetMapping("/getLogin")
    public JsonResult getLogin(@Validated User user, BindingResult bindingResult){
        if(bindingResult.hasErrors()){
            return JsonResult.fail(bindingResult.getFieldError().getDefaultMessage());
        }
        return JsonResult.success("登陆成功");
    }

```

![结果1.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTU0NWRmMjMzNjlmYTVhYTMucG5n?x-oss-process=image/format,png)

![结果2.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTM2ZDYwMWM2ZDA5NDE0YmQucG5n?x-oss-process=image/format,png)

### 统一的异常处理
```
package com.example.demo.exception;

import com.example.demo.result.JsonResult;
import org.springframework.validation.BindException;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import javax.validation.ConstraintViolationException;

@RestControllerAdvice
public class GlobalExceptionHandler {

    /**
     * post请求参数校验抛出的异常
     *
     * @param e
     * @return
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public JsonResult methodArgumentNotValidExceptionHandler(MethodArgumentNotValidException e) {
        //获取异常中随机一个异常信息
        System.out.println("methodArgumentNotValidExceptionHandler");
        String defaultMessage = e.getBindingResult().getFieldError().getDefaultMessage();
        return JsonResult.fail(defaultMessage);
    }


    /**
     * get请求参数校验抛出的异常
     *
     * @param e
     * @return
     */
    @ExceptionHandler(BindException.class)
    public JsonResult bindExceptionHandler(BindException e) {
        System.out.println("bindExceptionHandler");
        //获取异常中随机一个异常信息
        String defaultMessage = e.getBindingResult().getFieldError().getDefaultMessage();
        return JsonResult.fail(defaultMessage);
    }

    /**
     * 请求方法中校验抛出的异常
     *
     * @param e
     * @return
     */
    @ExceptionHandler(ConstraintViolationException.class)
    public JsonResult constraintViolationExceptionHandler(ConstraintViolationException e) {
        //获取异常中第一个错误信息
        System.out.println("constraintViolationExceptionHandler");
        String message = e.getConstraintViolations().iterator().next().getMessage();
        return JsonResult.fail(message);
    }

}

```
###  常用校验注解
```
@Null  被注释的元素必须为null
@NotNull  被注释的元素不能为null
@AssertTrue  被注释的元素必须为true
@AssertFalse  被注释的元素必须为false
@Min(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@Max(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@DecimalMin(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值
@DecimalMax(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值
@Size(max,min)  被注释的元素的大小必须在指定的范围内。
@Digits(integer,fraction)  被注释的元素必须是一个数字，其值必须在可接受的范围内
@Past  被注释的元素必须是一个过去的日期
@Future  被注释的元素必须是一个将来的日期
@Pattern(value) 被注释的元素必须符合指定的正则表达式。
@Email 被注释的元素必须是电子邮件地址
@Length 被注释的字符串的大小必须在指定的范围内
@NotEmpty  被注释的字符串必须非空
@Range  被注释的元素必须在合适的范围内
```

### 自定义校验
有时候需求复杂,需要自己去写一个

```
package com.example.demo.verify;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.*;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

@Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE})
@Retention(RUNTIME)
//用于校验手机号的逻辑类
@Constraint(validatedBy = PhoneValidator.class)
public @interface Phone {
    //手机号的校验格式
    String regexp() default "^[1][3-9][0-9]{9}$";

    //出现错误返回的信息
    String message() default "手机号格式错误";

    Class<?>[] groups() default { };

    Class<? extends Payload>[] payload() default { };

    @Target({ METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE })
    @Retention(RUNTIME)
    @Documented
    public @interface List {
        Phone[] value();
    }
}
```
```
package com.example.demo.verify;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

//校验注解的类必须实现ConstraintValidator，第一个泛型是注解，第二个泛型是校验参数的类型（手机号是String类型）
public class PhoneValidator implements ConstraintValidator<Phone, String> {

    private String regexp;

    //初始化方法
    @Override
    public void initialize(Phone constraintAnnotation) {
        //获取校验的手机号的格式
        this.regexp = constraintAnnotation.regexp();
    }

    //value是@Phone注解所注解的字段值
    //校验，返回true则通过校验，返回false则校验失败，错误信息为注解中的message
    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) {
            return true;
        }
        return value.matches(regexp);
    }
}
```
```
package com.example.demo;

import com.example.demo.result.JsonResult;
import com.example.demo.verify.Phone;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.validation.constraints.NotNull;

@RestController
@Validated
public class PhoneController {

    @GetMapping("/sendPhone")
    public JsonResultsendPhone(@Phone @NotNull(message = "手机号不能为空") String phone) {
        return JsonResult.success("正确的手机号");
    }
}
```
![自定义校验测试结果.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LTE3ZTM1NDA2ZWZiYzMwZTUucG5n?x-oss-process=image/format,png)

### 分组校验 

在上面的测试用户类新增两个接口
```
   public interface CheckName {
    }

    public interface CheckPassword {
    }
```
修改属性的groups
```
 @Length(min = 5, max = 10, message = "用户名长度不合法", groups = CheckName.class)
    @NotNull(message = "用户名不能为空")
    private String username;

    @Length(min = 6, max = 16, message = "密码长度不合法", groups = CheckPassword.class)
    @NotNull(message = "密码不能为空")
    private String password;
```
修改UserController类下的login进行测试
```
    @PostMapping("/login")
    public JsonResult login(@Validated(value = User.CheckName.class) @RequestBody User user) {
        return JsonResult.success("登陆成功");
    }
```
![分组校验.png](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80ODMwNjY0LWFkMTliM2EzN2RmYmU2NGYucG5n?x-oss-process=image/format,png)

我们会发现密码的校验没有进行 




