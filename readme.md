# IDEA SpringBoot多模块项目搭建详细过程

### Reference Documentation
1. 项目介绍： 

本项目包含一个父工程 demo 和 四 个子模块（demo-base, demo-dao, demo-service, demo-web)， 
demo-base 为其他三个模块的公共内容， 四个模块都依赖父模块， 
demo-dao 依赖 demo-base；   
demo-service 依赖 demo-dao，也就间接依赖 demo-base;   
demo-web 依赖 demo-service, 也就间接依赖 demo-base和demo-dao

2. 打包注意事项
①正常打JAR包：找到父模块，执行clean，执行package进行打包
②异常到JAR包：需要除去test测试，去dos命令行运行命令
父模块下运行命令:mvn clean package -Dmaven.test.skip=true
项目打包遇到的问题：
* 1. 打包时使用：mvn clean package -Dmaven.test.skip=true 忽略测试进行打包。测试代码不会影响项目发布，但是会影响项目的打包。
* 2. 项目打包报错：Non-resolvable parent POM for demo:demo-base:0.0.1-SNAPSHOT: Could not find artifact 
      demo:demo:pom:0.0.1-SNAPSHOT and 'parent.relativePath' points at no local POM
    解决方法：修改父项目，子Module：demo-base,demo-dao,demo-service,demo-web的 pom 文件，删除<parent></parent>标签的这句 <relativePath/> <!-- lookup parent from repository -->
* 3. 项目打包报错：[ERROR] Failed to execute goal org.springframework.boot:spring-boot-maven-plugin:1.5.16.RELEASE:
      repackage (default) on project demo-base: Execution default of goal org.springframework.boot:spring-boot-maven-plugin:1.5.16.RELEASE:repackage failed: Unable to find main class -> [Help 1]
    原因：这是 demo-base 找不到主类的问题。因为此项目为多模块项目，但并不是每一个子模块都需要打成可执行的 Jar 包，如此项目中实际需要打成可执行的Jar 包的只有demo-web，且我们在构建项目之前已经将出demo-web外的项目的启动类即主类删了。所以此处会报错。
      解决方法：修改除demo-web外的项目：父项目demo，demo-base、demo-dao、demo-service 的 pom 文件，删除其中关于打包的配置
3. 打包之后运行JAR
 此时可以在我们的启动模块下target文件夹下找到打成的jar包
 可以用命令到文件夹下启动jar文件
 java -jar xxx.jar(文件名)
 也可以在idea中直接右键启动   
### 参考引文文档
The following guides illustrate how to use some features concretely:

* https://blog.csdn.net/zcf980/article/details/83040029
* https://blog.csdn.net/wanghantong/article/details/36427411
* https://blog.csdn.net/weixin_44550842/article/details/100583525
* https://blog.csdn.net/baidu_41885330/article/details/81875395?utm_source=distribute.pc_relevant.none-task
