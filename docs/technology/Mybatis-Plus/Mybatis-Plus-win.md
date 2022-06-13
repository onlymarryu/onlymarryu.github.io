# Mybatis-Plus-win

# Mybatis-Plus

#  简介

[MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis (opens new window)](https://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

> 愿景
>
> 我们的愿景是成为 MyBatis 最好的搭档，就像 [魂斗罗](https://baomidou.com/img/contra.jpg) 中的 1P、2P，基友搭配，效率翻倍。

## 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

## [](https://baomidou.com/pages/24112f/#支持数据库)支持数据库

> 任何能使用 `MyBatis` 进行 CRUD, 并且支持标准 SQL 的数据库，具体支持情况如下，如果不在下列表查看分页部分教程 PR 您的支持。

- MySQL，Oracle，DB2，H2，HSQL，SQLite，PostgreSQL，SQLServer，Phoenix，Gauss ，ClickHouse，Sybase，OceanBase，Firebird，Cubrid，Goldilocks，csiidb
- 达梦数据库，虚谷数据库，人大金仓数据库，南大通用(华库)数据库，南大通用数据库，神通数据库，瀚高数据库

## [](https://baomidou.com/pages/24112f/#框架结构)框架结构

![framework](https://baomidou.com/img/mybatis-plus-framework.jpg)



# 快速使用

> 使用的mybatis 3.5之前版本，也就是官方的旧版本

## 初始化工程

创建一个空的 Spring Boot 工程（工程将以 H2 作为默认数据库进行演示）

提示

可以使用 [Spring Initializer (opens new window)](https://start.spring.io/)快速初始化一个 Spring Boot 工程



## `pom` 添加依赖

```xml

        <!--模板引擎,也可使用freemarker-->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.3</version>
        </dependency>

        <!--代码生成器 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>

        <!--内嵌mybatis，不需要在添加-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.1</version>
        </dependency>


        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>

        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.3</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>


```







## 逆向工程

测试用例

```java
import com.baomidou.mybatisplus.core.toolkit.StringPool;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.InjectionConfig;
import com.baomidou.mybatisplus.generator.config.*;
import com.baomidou.mybatisplus.generator.config.po.TableInfo;
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;
import java.util.Properties;


public class Generator {

    public static void main(String[] args) {
        //0自动生成
        AutoGenerator autoGenerator = new AutoGenerator();

        //1 数据源配置
        DataSourceConfig datasource = new DataSourceConfig();
        datasource.setUrl("jdbc:mysql://127.0.0.1:3306/xhshixun?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai");
        // dsc.setSchemaName("public");
        datasource.setDriverName("com.mysql.jdbc.Driver");
        datasource.setUsername("root");
        datasource.setPassword("root");

        autoGenerator.setDataSource(datasource);

        // 2全局配置
        GlobalConfig gc = new GlobalConfig();
        /**
         * System.getProperty(key)
         * getProperty()这个方法是获取指定键指示的系统属性的。
         * 如果key不是系统属性本来系统自带的，需要用户自己设定采用。如果不设定，则为null
         */
        String projectPath = System.getProperty("user.dir");
        //与其等同  ||
        // String projectPath = "E:\\code\\java\\test";

        //生成在那个目录下
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setOpen(false);//生成之后打开目录？是在资源管理器中打开的
        gc.setAuthor("zjj");//作者
        gc.setFileOverride(true);//是否覆盖原来文件
        //gc.setMapperName("%sDao");//设置数据层接口名，%s指代模块名,默认是*Mapper
        gc.setMapperName("%sMapper");
        // ,,,其余自行探索
//        gc.setSwagger2(true);// 实体属性 Swagger2 注解
        autoGenerator.setGlobalConfig(gc);

        //3 包配置
        PackageConfig pc = new PackageConfig();
        //pc.setModuleName(null); //模块名
        pc.setParent("com.aaa");
        pc.setEntity("pojo");   //设置实体类包名
        pc.setMapper("mapper"); //设置mapper层包名
        //,,,其余自行探索
        autoGenerator.setPackageInfo(pc);

        //4自定义配置
        // 如果模板引擎是 freemarker
        //String templatePath = "/templates/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        //String templatePath = "/templates/mapper.xml.vm";
        String templatePath = "/templates/service.java.vm";



        // 4自定义配置
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };
        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/mapper/" + pc.getModuleName()
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        cfg.setFileOutConfigList(focList);
        autoGenerator.setCfg(cfg);

        // 5配置模板
        TemplateConfig templateConfig = new TemplateConfig();

        // 配置自定义输出模板
        //指定自定义模板路径，注意不要带上.ftl/.vm, 会根据使用的模板引擎自动识别
        // templateConfig.setEntity("templates/entity2.java");
        // templateConfig.setService();
        // templateConfig.setController();

        templateConfig.setXml(null);
        autoGenerator.setTemplate(templateConfig);

        //提交配置
        autoGenerator.setPackageInfo(pc);


        //6策略设置
        StrategyConfig strategyConfig = new StrategyConfig();

        //strategyConfig.setInclude("tbl_user");//设置当前参与生成的表名，参数为可变参数
        strategyConfig.setTablePrefix("tbl_");//设置数据库表的前缀，模块名=数振库表名+前缀名

//        strategyConfig.setRestControllerStyle(true);//设置是否启用Rest风格
//        strategyConfig.setVersionFieldName("version");//设置乐观锁宁段名
//        strategyConfig.setLogicDelet  eFieldName("deleted");//设置逻辑删除字段将
//        strategyConfig.setEntityLombokModel(true);//设置是否启用Lombok
//        strategyConfig.setControllerMappingHyphenStyle(true); //url中驼峰转连字符

        autoGenerator.setStrategy(strategyConfig);

        //执行生成操作
        autoGenerator.execute();
    }

    @Test
    public void test(){
        //获取所有的属性
        Properties properties = System.getProperties();
        //为了看看系统的属性有几个，加了一个计数器
        int count = 0;
        //遍历所有的属性
        for (String key : properties.stringPropertyNames()) {
            System.out.println(key + "=" + properties.getProperty(key));
            count++;
            if (key.equalsIgnoreCase("jdbc.drivers")){
                System.out.println("YES");
                return ;
            }

        }
        System.out.println(count);
    }
}

```




