# spring MVC使用说明书

## 重定向

```java
// 1. 对于 RestController 不起作用，因为RestController等同于加了 @ResponseBody注解
public String updateOrAddProject() {
   return "redirect:相对网址 或 绝对网址?参数名="+参数值;
}
// 2.1 只能重定向到相对路径
public ModelAndView findProjectPage() {
    ModelAndView modelAndView = new ModelAndView(需要跳转的页面路径);
    return modelAndView;
}
// 2.2
public RedirectView findProjectPage() {
    return new RedirectView(绝对路径，需要加http://);
}
// 4. 会抛异常
public void testRed(HttpServletResponse response) throws Exception{
	response.sendRedirect("https://www.baidu.com");
}
```

