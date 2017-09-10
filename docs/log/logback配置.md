[logback配置详解](http://www.jianshu.com/p/1ded57f6c4e3)

[logback高级特性AsyncAppender](http://blog.csdn.net/chenjie2000/article/details/8902727)

~~~xml
<configuration>

    <property name="log.dir" value="/test/logs"/>
    <property name="projectname" value="test"/>

    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d [%thread] %-5p [%c] [%F:%L] - %msg%n</pattern>
        </encoder>
    </appender>

	<appender name="OperationRollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.dir}/${projectname}_info.log</file>
        <!--历史日志处理-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.dir}/logs/${projectname}_info-%d{yyyy-MM-dd}.log</fileNamePattern>
            <!--日志保留30天-->
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder charset="GBK">
            <pattern>%d [%thread] %-5p [%c :%L] - %msg%n</pattern>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印info日志 -->
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <appender name="ERROR" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.dir}/${projectname}_error.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${log.dir}/logs/${projectname}_error-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder charset="GBK">
            <pattern>%d [%thread] %-5p [%c :%L] - %msg%n</pattern>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.LevelFilter"><!-- 只打印错误日志 -->
            <level>ERROR</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
    </appender>

    <appender name="OperationRollingFileASYNC" class="ch.qos.logback.classic.AsyncAppender">
			<discardingThreshold>0</discardingThreshold>
			<queueSize>2048</queueSize>
			<appender-ref ref="OperationRollingFile" />
	</appender>

    <logger name="com.zhongan.cf" additivity="false">
        <level value="INFO" />
        <!--info、error日志存储不同文件中-->
        <appender-ref ref="OperationRollingFileASYNC"/>
        <appender-ref ref="ERROR"/>
        <appender-ref ref="STDOUT" />
    </logger>

    <root level="info">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="OperationRollingFileASYNC" />
    </root>
</configuration>
~~~
