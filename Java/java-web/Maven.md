[toc]

# Maven

## 下载 Maven

地址：https://maven.apache.org/

## 安装 Maven

- 配置环境变量

  > 首先配置 MAVEN_HOME 为 maven 安装目录，然后再 Path 中添加 %MAVEN_HOME%\bin
  >
  > maven 在命令行使用 mvn 表示，依赖于 JAVA_HOME，所以要想使用 maven，要先配置 JAVA_HOME 为 jdk 的安装目录

- 配置 本地仓库路径

  >在 maven 安装目录下的 conf 文件夹下有一个 setting.xml，在里面的 50+ 行可以看到一个 localRepository，将其配置为
  >
  ><localRepository>想要放到的路径</localRepository> 就可以了

- 配置镜像地址

  > 同样是在 maven 安装目录下的 conf 文件夹下的 setting.xml 下，在 160+ 行的位置有一个 mirrors，将其改成
  >
  > <mirror>
  >
  >    <id>aliyunmaven</id>
  >
  >    <mirrorOf>central</mirrorOf>
  >
  >    <name>aliyun central maven</name>
  >
  >    <url>https://maven.aliyun.com/repository/central</url>
  >
  >   </mirror>

- IDEA 继承 maven，在 IDEA 的设置中 build 栏目更改

## Maven 管理项目依赖

> 通过 项目目录下的 pow.xml 文件来管理依赖
>
> 可以在 https://mvnrepository.com/ 这里查询依赖

例子：

~~~xml
<dependencies>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.9</version>
        <!-- 排除依赖，排除依赖时不需要指定版本 -->
        <exclusions>
            <exclusion>
                <groupId>组织名</groupId>
                <artifactId>模块名</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
~~~

- `dependencies：依赖列表`

- `dependency：单个依赖`

- `groupId：组织名`

- `artifactId：模块名`‘

- `version：版本名`

- `scope：控制依赖作用范围`

  | scope值         | 主程序 | 测试程序 | 打包 ( 运行 ) |
  | --------------- | ------ | -------- | ------------- |
  | `compile(默认)` | Y      | Y        | Y             |
  | `test`          | N      | Y        | N             |
  | `provided`      | Y      | Y        | N             |
  | `runtime`       | N      | Y        | Y             |

- `excluesions：要排除的依赖列表`

- `exclusion：要排除的依赖`

## Maven 生命周期

> Maven 有三套独立的生命周期
>
> 在同一套生命周期中，当运行后面的阶段时，前面的阶段都会运行
>
> Maven 的生命周期是抽象的，在它的生命周期中完成的操作全部都是由插件来定义的
>
> Maven 的生命周期有很多，但是只需要重点关注这 6 个

- `clean：清理工作`
  - `clean：移除上一次构建生成的文件`
- `default：核心工作`
  - `compile：编译项目源代码`
  - `test：使用合适的单元测试框架运行测试(junit)`
  - `package：将编译后的文件打包，如 jar,war等`
  - `install：安装项目打本地仓库`
  - `deploy：将你负责的模块发布到你配置的仓库位置`
- `site：生成报告、发布站点等`

