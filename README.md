# druidDemo
druid数据库使用demo

一、下载druid-1.0.9.jar导入到项目
这一步你就去下载就行了，地址：http://central.maven.org/maven2/com/alibaba/druid/1.0.9/druid-1.0.9.jar
二、applicationContext.xml文件配置数据库连接
这好像和其他数据库连接池没多少区别，除了几个参数不同而已，我贴出我的配置
<!--master 配置数据源 -->
<bean id="masterDataSource" class="com.alibaba.druid.pool.DruidDataSource"
    init-method="init" destroy-method="close">
    <property name="driverClassName">
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property name="url">
        <value>jdbc:mysql://127.0.0.1:3306/springdemo?useUnicode=true&characterEncoding=UTF-8</value>
    </property>
    <property name="username">
        <value>root</value>
    </property>
    <property name="password">
        <value>123456</value>
    </property>

    <!-- 通过别名的方式配置扩展插件，常用的插件有：监控统计用的filter:stat 日志用的filter:log4j 防御sql注入的filter:wall -->
    <property name="filters" value="stat,log4j" />
    <!-- 最大并发连接数 -->
    <property name="maxActive" value="30" />
    <!-- 初始化连接数量 -->
    <property name="initialSize" value="5" />
    <!-- 配置获取连接等待超时的时间 -->
    <property name="maxWait" value="60000" />
    <!-- 最小空闲连接数 -->
    <property name="minIdle" value="5" />
    <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
    <property name="timeBetweenEvictionRunsMillis" value="60000" />
    <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
    <property name="minEvictableIdleTimeMillis" value="300000" />
    <property name="validationQuery" value="SELECT 'x'" />
    <property name="testWhileIdle" value="true" />
    <property name="testOnBorrow" value="false" />
    <property name="testOnReturn" value="false" />
    <property name="poolPreparedStatements" value="false" />
    <property name="maxOpenPreparedStatements" value="100" />
    <!-- 打开removeAbandoned功能(连接泄漏监测，怀疑存在泄漏之后再打开) -->
    <property name="removeAbandoned" value="true" />
    <!-- 1800秒，也就是30分钟 -->
    <property name="removeAbandonedTimeout" value="1800" />
    <!-- 关闭abanded连接时输出错误日志 -->
    <property name="logAbandoned" value="true" />
</bean>
上面的参数都可以根据自己的需要来调整。具体配置请访问我的 GitHub 示例https://github.com/mafly/SpringDemo/blob/master/WebContent/WEB-INF/applicationContext.xml
三、web.xml文件配置监控平台
<!-- 连接池 启用Web监控统计功能   start-->
<filter>
    <filter-name>DruidWebStatFilter</filter-name>
    <filter-class>com.alibaba.druid.support.http.WebStatFilter</filter-class>
    <init-param>
        <param-name>exclusions</param-name>
        <param-value>*.js,*mp3,*.swf,*.xls,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>DruidWebStatFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
<servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
    <init-param>  
        <!-- 允许清空统计数据 -->  
        <param-name>resetEnable</param-name>  
        <param-value>true</param-value>  
    </init-param>  
    <init-param>  
        <!-- 用户名 -->  
        <param-name>loginUsername</param-name>  
        <param-value>admin</param-value>  
    </init-param>  
    <init-param>  
        <!-- 密码 -->  
        <param-name>loginPassword</param-name>  
        <param-value>123456</param-value>  
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
</servlet-mapping>
<!-- 连接池 启用Web监控统计功能   end-->

项目访问地址:http://localhost:8080/druidDemo/druid/index.html
