# minio的一个基础使用案例：用户头像上传

[TOC]


## 一、minio下载安装（Windows）

**1. 下载minio服务端和客户端**

>[minio下载地址](https://min.io/download?license=agpl&platform=windows#/windows)

![](/Res/images/minio下载.png)

**2. 手动搭建目录**

```java
/minio
    /bin
        mc.exe
        minio.exe
    /data
    /logs
```
* 手动创建minio应用程序目录，如：E:\minio\bin
* 手动创建minio数据目录，如：E:\minio\data
* 手动创建minio日志目录，如：E:\minio\logs
* 然后将下载的`mc.exe`文件和`minio.exe`文件放入E:\minio\bin目录下

**3. 启动**

* 在bin目录下打开cmd
> 到bin目录下后，在地址栏输入cmd，然后回车

* 设置用户名
```java
setx MINIO_ROOT_USER minioadmin
```

* 设置用户密码
```java
setx MINIO_ROOT_PASSWORD minioadmin
```
* 启动minio服务
```java
E:\minio\bin\minio.exe server E:\minio\data --console-address ":9001" --address ":9000" > E:\minio\logs\minio.log
```

**4. 访问minio控制台**

* 在浏览器输入**服务器Ip** + **9001端口号**打开登录页面，然后使用前面步骤中设置的用户名和密码登录控制台

* 建立一个新的bucket
![](/Res/images/minio-bucket页面.png)
![](/Res/images/minio-createBucket.png)

* 修改桶的访问权限为public
![](/Res/images/minio-afterCreateBucket.png)
![](/Res/images/minio-editBucketPage.png)
![](/Res/images/minio-editBucket.png)
## 二、案例需求分析

* 例如在添加用户和修改用户的时候，此时可以在表单页面点击"+"号，然后选择要上传的用户图像。
![](/Res/images/minio-上传文件需求分析1.png)
* 选择完毕以后，那么此时就会请求后端上传文件接口，将图片的二进制数据传递到后端
* 后端需要将数据图片存储起来，然后给前端返回图片的访问地址，然后前端需要将图片的访问地址设置给sysUser用户数据模型
* 当用户点击提交按钮的时候，那么此时就会将表单进行提交，后端将数据保存起来即可
## 三、后端接口开发

基本目录结构
```java
/java
----/controller
--------FileUploadController.java
----/properties
--------MinioProperties.java
----/service
--------/impl
------------FileUploadServiceImpl.java
--------FileUploadService.java
----ManagerApplication.java
/resources
----application-dev.yml
```

* 在application-dev.yml中添加minio相关配置

![](/Res/images/minio-上传文件后端接口实现-minio配置.png)
```yml
# 自定义配置
project:
  minio:
    endpointUrl: http://127.0.0.1:9000
    accessKey: minioadmin
    secureKey: minioadmin
    bucketName: b2c-e-commerce
```

* 新建MinioProperties.java，minio所需参数实体类

![](/Res/images/minio-上传文件后端接口实现-minio所需参数实体类.png)
```java
@Data
@ConfigurationProperties(prefix = "project.minio")
public class MinioProperties {
    private String endpointUrl;
    private String accessKey;
    private String secureKey;
    private String bucketName;
}
```

* 修改启动类ManagerApplication，添加`@EnableConfigurationProperties`注解，激活配置属性绑定功能

![](/Res/images/minio-上传文件后端接口实现-启动类激活配置绑定.png%20.png)
```java
@EnableConfigurationProperties(value = { MinioProperties.class})
```

* service层接口及实现类

基本思路：

1. 在fileUpload方法中，首先根据`minioProperties`创建一个**MinioClient实例**，用于与MinIO服务器交互。

2. 检查指定的桶（bucket）是否存在。如果不存在，则通过makeBucket方法创建一个新的桶。

3. **生成存储对象的名称**：结合当前日期（格式为"yyyyMMdd"）和一个随机UUID作为前缀，再加上原始文件名，确保文件名的唯一性。

4. 使用PutObjectArgs构建上传对象的参数，包括桶名、文件输入流（从MultipartFile获取）、文件大小和对象名称（即文件路径）。

5. 调用`minioClient.putObject`执行文件上传操作。
最后，返回文件在MinIO服务器上的访问URL，以便用户可以访问上传的文件。

FileUploadService.java
```java
public interface FileUploadService {
    String fileUpload(MultipartFile multipartFile);
}
```
FileUploadServiceImpl.java
```java
@Service
public class FileUploadServiceImpl implements FileUploadService {

    @Autowired
    private MinioProperties minioProperties ;

    @Override
    public String fileUpload(MultipartFile multipartFile) {

        try {
            // 创建一个Minio的客户端对象
            MinioClient minioClient = MinioClient.builder()
                    .endpoint(minioProperties.getEndpointUrl())
                    .credentials(minioProperties.getAccessKey(), minioProperties.getSecureKey())
                    .build();

            // 判断桶是否存在
            boolean found = minioClient.bucketExists(BucketExistsArgs.builder().bucket(minioProperties.getBucketName()).build());
            if (!found) {       // 如果不存在，那么此时就创建一个新的桶
                minioClient.makeBucket(MakeBucketArgs.builder().bucket(minioProperties.getBucketName()).build());
            } else {  // 如果存在打印信息
                System.out.println("Bucket 'b2c-e-commerce' already exists.");
            }

            // 设置存储对象名称
            String dateDir = DateUtil.format(new Date(), "yyyyMMdd");
            String uuid = UUID.randomUUID().toString().replace("-", "");
            //20230801/443e1e772bef482c95be28704bec58a901.jpg
            String fileName = dateDir+"/"+uuid+multipartFile.getOriginalFilename();
            System.out.println(fileName);

            PutObjectArgs putObjectArgs = PutObjectArgs.builder()
                    .bucket(minioProperties.getBucketName())
                    .stream(multipartFile.getInputStream(), multipartFile.getSize(), -1)
                    .object(fileName)
                    .build();
            minioClient.putObject(putObjectArgs) ;

            return minioProperties.getEndpointUrl() + "/" + minioProperties.getBucketName() + "/" + fileName ;

        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

* controller层

```java
@RestController
@RequestMapping("/admin/system")
public class FileUploadController {

    @Autowired
    private FileUploadService fileUploadService ;

    @PostMapping(value = "/fileUpload")
    public Result<String> fileUploadService(@RequestParam(value = "file") MultipartFile multipartFile) {
        String fileUrl = fileUploadService.fileUpload(multipartFile) ;
        return Result.build(fileUrl , ResultCodeEnum.SUCCESS) ;
    }

}
```
