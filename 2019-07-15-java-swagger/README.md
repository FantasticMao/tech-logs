在前后端分离的应用日常开发中，开发人员可以使用 [Swagger](https://swagger.io) 编写接口文档。

Swagger 是一个用于描述和记录 RESTful API 的项目，Swagger 规范定义了描述 RESTful API 所需的一组文件，然后 Swagger-UI 可以使用这些文件来显示 API 文档、Swagger-Codegen 可以使用这些文件来生成各种语言的客户端。

在 [GitHub](https://github.com/swagger-api) 上，Swagger 的开源项目有：
* **[Swagger Editor](https://swagger.io/docs/open-source-tools/swagger-editor/)** 是一个开源编辑器，可以用于在 Swagger 规范中设计、定义和记录 RESTful API。
* **[Swagger UI](https://swagger.io/docs/open-source-tools/swagger-ui/)** 是一组 HTML、JavaScript、CSS 资源的集合，可以从符合 Swagger 规范的 API 中动态生成漂亮的文档。
* **[Swagger Codegen](https://swagger.io/docs/open-source-tools/swagger-codegen/)** 是一个开源的代码生成器，可以直接从 Swagger 定义的 RESTful API 中构建服务器存根和客户端 SDK。

更多信息请查看官方文档：https://docs.swagger.io/spec.html 。

---

在 Java 世界的 Spring 应用中，可以使用 [Springfox](http://springfox.io/) 集成使用 Swagger。

Springfox 最初是由 Marty Pitt 创建的项目 *swagger-springmvc* 发展而来，可以帮助集成了 Spring Framework 的相关应用便捷自动地生成机器和人类可读的接口文档，有助于开发人员维护应用的接口文档。

Springfox 的使用方式也很简单，下面举一个简单的例子：
1. 导入相关依赖
    ```xml
    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger2</artifactId>
    </dependency>

    <dependency>
        <groupId>io.springfox</groupId>
        <artifactId>springfox-swagger-ui</artifactId>
    </dependency>
    ```
2. 使用注解标识接口
    ```java
    @Api(tags = "用户相关接口")
    @RestController
    public class UserController {

        @ApiOperation(value = "获取用户数据")
        @GetMapping(value = "/getUser", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
        public User getUser(@ApiParam(value = "用户ID", example = "7", required = true)
                            @RequestParam Integer userId) {
            return new User("汤姆", 18);
        }

        @ApiModel
        public class User {
            @ApiModelProperty(value = "名称", example = "汤姆")
            private String name;
            @ApiModelProperty(value = "年龄", example = "18")
            private Integer age;

            public User(String name, Integer age) {
                this.name = name;
                this.age = age;
            }

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }

            public Integer getAge() {
                return age;
            }

            public void setAge(Integer age) {
                this.age = age;
            }
        }
    }
    ```
3. API 文档页面
    ![image](1.png)