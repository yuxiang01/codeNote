# 文件上传和下载
## 文件上传
[[十二、文件上传下载#1. 文件上传| 文件上传]]
### 文件上传开发步骤
1. 判断是不是文件表单`enctype = "multipart/form-data"`  
2. 创建`DiskFileItemFactory` 对象，用于构建一个解析上传数据的工具对象 
3. 创建一个解析上传数据的工具对象 
	- **设置请求头编码格式**,解决文件名中文乱码问题
4. 关键 
	1. 判断是不是文件
		- 如果是`true`就说明是文本 `input:text`
		- 获取文本信息
		`String name = fileItem.getString("utf-8");`
	1. 否则是文件
	    - 获取文件名 
	    - 把上传到服务器temp目录下的文件保存到指定目录
		    1. 指定目录 `/upload/` + [[WebUtils#1. 获取时间 | 年/月/日]]
		    2. 获取完整路径
			    `利用ServletContent应用实例: [获取项目发布后，正在工作的路径]`
		    3. 将文件拷贝到 `fileRealPathDirectory`
			    `防止同名文件覆盖问题，添加UUID作为前缀`
		    4. 创建目录`[ 不存在 ？ 创建 ：不创建 ]` 
5. 提示信息 
### 文件上传源码
```html
<form action="fileUpload" method="post" enctype="multipart/form-data">  
  选择图片: <input type="file" name="img" multiple>  
  <input type="submit">  
</form>
```

```java
@WebServlet(name = "FileUploadServlet", value = "/fileUpload")  
public class FileUploadServlet extends HttpServlet {  
  @Override  
  protected void doGet(HttpServletRequest request, HttpServletResponse response) 
  throws ServletException, IOException {  
    // 1. 判断是不是文件表单（enctype = "multipart/form-data"）  
    if (ServletFileUpload.isMultipartContent(request)) {  
      // 2. 创建 DiskFileItemFactory 对象，用于构建一个解析上传数据的工具对象  
      DiskFileItemFactory diskFileItemFactory = new DiskFileItemFactory();  
      // 3. 创建一个解析上传数据的工具对象  
      ServletFileUpload servletFileUpload = 
	      new ServletFileUpload(diskFileItemFactory);  
	  // 解决文件名中文乱码问题  
	  servletFileUpload.setHeaderEncoding("utf-8");
      // 4.关键  
      try {  
        List<FileItem> list = servletFileUpload.parseRequest(request);  
        for (FileItem fileItem : list) {  
          // System.out.println(list);  
          // 判断是不是文件  
          if (fileItem.isFormField()) {  
            // 如果是true就说明是文本 input text
            String name = fileItem.getString("utf-8");  
            System.out.println("家具名 = " + name);  
          } else {  
            // 获取文件名  
            String name = fileItem.getName();  
            System.out.println("文件名 = " + name);  
            // 把这个上传到服务器temp目录下的文件保存到指定目录  
            // 1. 指定目录  
            String filePath = "/upload/";  
            // 2. 获取完整路径  
            // ServletContext应用实例: [获取项目发布后，正在工作的路径]  
	        // System.out.println(request.getServletContext().getRealPath("/"));
            String realPath = request.getServletContext().getRealPath(filePath);  
            System.out.println("真实路径：" + realPath);  
            // 3. 创建目录 [ 不存在 ？ 创建 ：不创建 ]            
            File fileRealPathDirectory 
	            = new File(realPath+ WebUtils.getYearMonthDay());  
            if (!fileRealPathDirectory.exists()) {  
              fileRealPathDirectory.mkdirs();  
            }  
            // 防止同名文件覆盖问题，添加UUID作为前缀  
			name = UUID.randomUUID() + "_" + name;
            // 4. 将文件拷贝到 fileRealPathDirectory 
            String fileFullPath = fileRealPathDirectory + "/" + name;  
            fileItem.write(new File(fileFullPath));  
            // 5. 提示信息  
            response.setContentType("text/html;charset=utf-8");  
            response.getWriter().write("文件上传，成功！");  
          }  
        }      
	  } catch (Exception e) {  
	    e.printStackTrace();  
      }  
    } else {  
      System.out.println("fail");  
    }  
  }
}
```

## 文件下载
[[十二、文件上传下载#2. 文件下载 | 文件上传]]
### 文件下载开发步骤
1. 先准备要下载的文件【必须保证tomcat启动后，工作目录中有文件夹】 rebuild -> restart
2. 获取到要下载的文件名
3. 给http响应，设置响应头Content-Type，就是文件Mine类型
	- 如何获取Mine类型？
		- => 通过servletContext来获取
		- 先确认下载根目录 `/public/`
		- 再拼接完整路径名 `/public/图片.jpg`
		- 通过完整路径名,使用ServletContext的方法getMimeType,获取Mime类型
	- 设置响应ContentType
4. 给http响应，设置响应头Content-Disposition
	- 针对不同浏览器，对下载时，显示文件名进行编码url和火狐base64
5. 读取下载的文件数据，返回给客户端/浏览器 
	(1). 创建一个和要下载的文件，相关联的输入流
	(2). 得到返回数据的输出流【因为返回文件大多是二进制】
	(3). 使用工具类，将输入流关联的文件，对拷到输出流，并返回给客户端
### 文件下载源码
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>  
<html>  
<head>  
  <meta charset="UTF-8">  
  <title>文件下载</title>  
  <base href="<%=request.getContextPath()%>/">  
</head>  
<body>  
<h1>文件下载</h1>  
<a href="down?name=ok.jpeg">下载图片</a><br/><br/>  
<a href="down?name=算法图解.pdf">下载算法图解.pdf</a><br/><br/>  
</body>  
</html>
```


```java
@WebServlet(name = "DownServlet", value = "/down")  
public class DownServlet extends HttpServlet {  
  @Override  
  protected void doPost(HttpServletRequest request, HttpServletResponse response)  
      throws ServletException, IOException {  
    // 1. 先准备要下载的文件【必须保证tomcat启动后，工作目录中有文件夹】 rebuild -> restart    
    // 2. 获取到要下载的文件名  
    request.setCharacterEncoding("utf-8");  
    String fileDownName = request.getParameter("name");  
    System.out.println(fileDownName);  
    // 3.给http响应，设置响应头Content-Type，就是文件Mine类型  
    // 如何获取Mine类型？-> 通过servletContext来获取  
    ServletContext servletContext = request.getServletContext();  
    String downLoad = "/public/";  
    String downLoadFullPath = downLoad + fileDownName;  
    String mimeType = servletContext.getMimeType(downLoadFullPath);  
    // 设置响应ContentType  
    response.setContentType(mimeType);  
    // 4. 给http响应，设置响应头Content-Disposition  
    // 针对不同浏览器，对下载时，显示文件名进行编码url和火狐[base64]  
    if (request.getHeader("User-Agent").contains("Firefox")) {  
      response.setHeader("Content-Disposition", "attachment;filename==?UTF-8?B?"  
          + new BASE64Encoder().encode(downLoadFullPath.getBytes("UTF-8")) + "?="  
      );  
    } else {  
      response.setHeader("Content-Disposition",  
          "attachment;filename=" + URLEncoder.encode(downLoadFullPath, "UTF-8"));  
    }  
    // 5. 读取下载的文件数据，返回给客户端/浏览器  
    // (1). 创建一个和要下载的文件，相关联的输入流  
    InputStream resourceAsStream = servletContext.getResourceAsStream(downLoadFullPath);  
    // (2). 得到返回数据的输出流【因为返回文件大多是二进制】  
    ServletOutputStream outputStream = response.getOutputStream();  
    // (3). 使用工具类，将输入流关联的文件，对拷到输出流，并返回给客户端  
    IOUtils.copy(resourceAsStream, outputStream);  
  }
}
```

