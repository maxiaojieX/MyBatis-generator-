## MyBatis逆向工程(generator)
* 1、配置xml文件(generatorConfig.xml)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--使用本地connector.jar连接数据库-->
    <classPathEntry location="F:\maven_local_lib\mysql\mysql-connector-java\5.1.26\mysql-connector-java-5.1.26.jar"/>
    <!--targetRuntime:MyBatis3:生成带有Example
                      MyBatis3Simple：生成简单的类和sql语句-->
    <context id="myConfig" targetRuntime="MyBatis3">
        <!--是否禁用默认注释-->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
            <property name="suppressDate" value="true"/>
        </commentGenerator>
        <!--连接数据库-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="123456"/>
        <!--targetPackage：生成的实体类放进哪个包？  targetProject：包的位置？-->
        <javaModelGenerator targetPackage="com.kaishengit.entity" targetProject="src/main/java"/>
        <!--targetPackage：生成的XML放在哪个文件夹？  targetProject：文件夹的位置 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources"/>
        <!--targetPackage：生成的Mapper接口放在哪个包？ targetProject：包的位置？-->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.kaishengit.mapper" targetProject="src/main/java"/>
        <!--tableName:为数据库中哪些表使用自动生成  domainObjectName:在JAVA中的名字-->
        <table tableName="t_project" domainObjectName="Project" enableSelectByExample="true"/>
    </context>
</generatorConfiguration>
```
* 2、对于Example的使用
    ##### 每一个Example类中，都为实体类中的每个属性(字段)建立了对应的In EqualsTo LessThan...方法，我们可以调用这些方法解决一些稍微复杂的SQL语句,例如：
    ```java
    public void selectByOther(){
        SqlSession sqlSession = MyBatisUtil.getSqlSessionFactory().openSession();
        ArticleMapper articleMapper = sqlSession.getMapper(ArticleMapper.class);

        //把对用的实体类Example new出来
        ArticleExample articleExample = new ArticleExample();
        //Nodeid是Article表中的某个字段
        articleExample.createCriteria().andNodeidEqualTo(1);
        //调用了select方法，那么生成的sql语句应该是 select * from t_article where nodeid=1
        List<Article> articleList = articleMapper.selectByExample(articleExample);
        
        for(Article article : articleList){
            System.out.println(article);
        }
    }
    ```