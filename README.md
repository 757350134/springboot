# springboot
spring boot读取配置文件

1.环境  win10  eclipse3.4 jdk7  maven3.3

2.dependency

 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>1.3.2.RELEASE</version>
 </dependency>

3.默认配置文件名  application.properties
server.info.address=192.168.1.1
server.info.username=user1
server.info.password=password

一般配置文件名 server.properties
server.info.address=192.168.1.2
server.info.username=user2
server.info.password=password2

4.java代码
application.properties读取类
@Configuration
@ConfigurationProperties(prefix = "server.info")
public class DefaultProperties {
    private String address;
    private String username;
    private String password;

    //getter and setter methods

    @Override
    public String toString() {
        return "DefaultProperties{" +
                "address='" + address + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
@ConfigurationProperties告诉spring去绑定properties, prefix 代表配置文件变量前缀

server.properties读取类
@Configuration
@ConfigurationProperties(locations="classpath:server.properties",prefix = "server.info")
public class SpecialProperties {
    private String address;
    private String username;
    private String password;

    //getter and setter methods

    @Override
    public String toString() {
        return "SpecialProperties{" +
                "address='" + address + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}

5.最后，通过controller启动程序
@RestController
@EnableAutoConfiguration
@ComponentScan(value = "com.cntv.propertiestest")
public class SimpleController {
    @Autowired
    private DefaultProperties defaultProperties;

    @Autowired
    private SpecialProperties specialProperties;

    @RequestMapping(value = "/properties", method = RequestMethod.GET)
    public String getProperties() {
        return defaultProperties.toString() + "<br>" + specialProperties.toString();
    }

    public static void main(String[] args) {
        SpringApplication.run(SimpleController.class, args);
    }
}

6.
测试：启动SimpleController  main方法无报错  则
浏览器输入 localhost:8080/properties

浏览器结果：
DefaultProperties [userName=user1, password=password, address=192.168.1.1]
SpecialProperties{address='192.168.1.2', username='user2', password='password2'}
