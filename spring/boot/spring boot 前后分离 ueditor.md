# spring boot 前后分离 ueditor

[TOC]

# 参考资料

[Springboot读取本地图片并显示](https://www.cnblogs.com/yuxifly828/p/9732911.html)

> 在 application.yml 中增加如下配置
> ```yml
> web:
>   upload-path: upload
> 
> spring:
>   mvc:
>     static-path-pattern: /upload/**
>   resources:
>     static-locations: file:${web.upload-path}
> ```
> 

[vue+Ueditor集成 [前后端分离项目][图片、文件上传][富文本编辑]](https://www.bbsmax.com/A/xl56mk0kJr/)

> 孙梦提供的配置方法
>
> 
>
> 在config层中，新建一个类 CrosConfig.java
>
> ```java
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> import org.springframework.web.cors.CorsConfiguration;
> import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
> import org.springframework.web.filter.CorsFilter;
> 
> /**
>  * 跨域请求处理
>  *
>  * @author Guoqing
>  */
> @Configuration
> public class CrosConfig {
> 
>     private CorsConfiguration buildConfig() {
>         CorsConfiguration corsConfiguration = new CorsConfiguration();
>         corsConfiguration.addAllowedOrigin("*"); // 1
>         corsConfiguration.addAllowedHeader("*"); // 2
>         corsConfiguration.addAllowedMethod("*"); // 3
>         return corsConfiguration;
>     }
> 
>     @Bean
>     public CorsFilter corsFilter() {
>         UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
>         source.registerCorsConfiguration("/**", buildConfig()); // 4
>         return new CorsFilter(source);
>     }
> 
> }
> ```
>
> 
>
> 在control层中，新建一个类 UeditorController.java
>
> ```java
> import com.wdzggroup.rbzy.oa.common.ueditor.ActionEnter;
> import org.springframework.web.bind.annotation.RequestMapping;
> import org.springframework.web.bind.annotation.ResponseBody;
> import org.springframework.web.bind.annotation.RestController;
> 
> import javax.servlet.http.HttpServletRequest;
> import java.io.UnsupportedEncodingException;
> 
> @RestController
> @RequestMapping("/ueditor")
> public class UEditorController {
>     @RequestMapping(value = "/exec")
>     @ResponseBody
>     public String exec(HttpServletRequest request) throws UnsupportedEncodingException {
>         request.setCharacterEncoding("utf-8");
>         String rootPath = request.getServletContext().getRealPath("/");
>         return new ActionEnter(request, rootPath).exec();
>     }
> }
> ```
>
> 

[vue + Springboot 前后端分离集成UEditor](https://blog.csdn.net/qq_38960998/article/details/83038082)

> 个人觉得这篇文章在使用中最为靠谱
>
> 
>
> 首先从官网上下载源代码，下载之后解压缩，将jsp目录下的src中代码复制到项目中，然后将 config.json 放到项目的 resources 中
>
> 修改 ConfigManage.java 类的 getConfigPath() 方法，使其能加载 config.json 文件
>
> ```java
> private String getConfigPath() throws FileNotFoundException{
>     return ResourceUtils.getFile("classpath:config.json").getAbsolutePath();
> }
> ```
>
> 修改 BinaryUploader 类的 public static final State save(HttpServletRequest request, Map<String, Object> conf) 方法
>
> ```java
> public static final State save(HttpServletRequest request, Map<String, Object> conf) {
>     boolean isAjaxUpload = request.getHeader("X_Requested_With") != null;
> 
>     if (!ServletFileUpload.isMultipartContent(request)) {
>         return new BaseState(false, AppInfo.NOT_MULTIPART_CONTENT);
>     }
> 
>     ServletFileUpload upload = new ServletFileUpload(
>             new DiskFileItemFactory());
> 
>     if (isAjaxUpload) {
>         upload.setHeaderEncoding("UTF-8");
>     }
> 
>     try {
>         MultipartHttpServletRequest multipartRequest = (MultipartHttpServletRequest) request;
>         MultipartFile fileStream = multipartRequest.getFile("upfile");
> 
>         if (fileStream == null) {
>             return new BaseState(false, AppInfo.NOTFOUND_UPLOAD_DATA);
>         }
> 
>         String savePath = (String) conf.get("savePath");
>         String originFileName = fileStream.getOriginalFilename();
> 
>         if (originFileName == null) {
>             return new BaseState(false, AppInfo.PARSE_REQUEST_ERROR);
>         }
> 
>         String suffix = FileType.getSuffixByFilename(originFileName);
> 
>         originFileName = originFileName.substring(0,
>                 originFileName.length() - suffix.length());
>         savePath = savePath + suffix;
> 
>         long maxSize = ((Long) conf.get("maxSize")).longValue();
> 
>         if (!validType(suffix, (String[]) conf.get("allowFiles"))) {
>             return new BaseState(false, AppInfo.NOT_ALLOW_FILE_TYPE);
>         }
> 
>         savePath = PathFormat.parse(savePath, originFileName);
> 
>         String physicalPath = (String) conf.get("uploadPath") + savePath;
> 
>         InputStream is = fileStream.getInputStream();
>         State storageState = StorageManager.saveFileByInputStream(is,
>                 physicalPath, maxSize);
>         is.close();
> 
>         if (storageState.isSuccess()) {
>             storageState.putInfo("url", PathFormat.format(physicalPath));
>             storageState.putInfo("type", suffix);
>             storageState.putInfo("original", originFileName + suffix);
>         }
> 
>         return storageState;
>     } catch (IOException e) {
>     }
>     return new BaseState(false, AppInfo.IO_ERROR);
> }
> ```
>
> 在 config.json 中加入如下代码
>
> ```json
> /* 上传图片配置项 */
> "uploadPath": "upload", /* ---------- 仅这行是新增的 ---------- */
> "imageActionName": "uploadimage", /* 执行上传图片的action名称 */
> "imageFieldName": "upfile", /* 提交的图片表单名称 */
> "imageMaxSize": 2048000, /* 上传大小限制，单位B */
> ```
>
> 在 ConfigManager.java 类的 Map<String, Object> getConfig(int type) 方法中加入如下代码
>
> ```java
> conf.put("uploadPath", this.jsonConfig.getString("uploadPath")); // 仅这行是新增的
> conf.put("savePath", savePath);
> conf.put("rootPath", this.rootPath);
> ```
>
> 