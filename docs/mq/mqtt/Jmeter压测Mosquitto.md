# windows部署Jmeter
直接去官网下载apache-jmeter-3.2.zip，解压、运行bin/jmeter.bat即可

# mqtt-Jmeter插件
JMeter作为现在最流行的开源性能测试工具，实现了对各种协议的测试支持，比如HTTP／HTTPS，SOAP／REST，FTP，JDBC等。JMeter提供了灵活的插件机制，允许第三方扩展不支持的协议，网站 https://jmeter-plugins.org/ 中搜集了将近60多个插件，这些插件提供了各种功能，但是该网站并没有提供测试MQTT的插件。

[github tuanhiep](https://github.com/tuanhiep/mqtt-jmeter?spm=5176.100239.blogcont74411.13.GCV6K0)是一个不错的插件，基本的MQTT测试功能都已经提供。具体使用见该项目的github。

我在使用中遇到如下问题：
~~~java
ERROR - jmeter.threads.JMeterThread: Test failed! java.lang.NoClassDefFoundError: org/fusesource/mqtt/client/FutureConnection
at org.apache.jmeter.protocol.mqtt.client.MqttPublisher.setupTest(MqttPublisher.java:98)
at org.apache.jmeter.protocol.mqtt.client.MqttPublisher.setupTest(MqttPublisher.java:79)
at org.apache.jmeter.protocol.mqtt.sampler.PublisherSampler.threadStarted(PublisherSampler.java:419)
at org.apache.jmeter.threads.JMeterThread$ThreadListenerTraverser.addNode(JMeterThread.java:599)
at org.apache.jorphan.collections.HashTree.traverseInto(HashTree.java:961)
at org.apache.jorphan.collections.HashTree.traverse(HashTree.java:946)
at org.apache.jmeter.threads.JMeterThread.threadStarted(JMeterThread.java:568)
at org.apache.jmeter.threads.JMeterThread.initRun(JMeterThread.java:556)
at org.apache.jmeter.threads.JMeterThread.run(JMeterThread.java:254)
at java.lang.Thread.run(Unknown Source)
~~~
这是由于mvn clean install package打包时未将引用的第三方包包含进去，修改pom.xml，重新打包：
~~~xml
<!--
 Licensed to the Apache Software Foundation (ASF) under one
 or more contributor license agreements.  See the NOTICE file
 distributed with this work for additional information
 regarding copyright ownership.  The ASF licenses this file
 to you under the Apache License, Version 2.0 (the
 "License"); you may not use this file except in compliance
 with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing,
 software distributed under the License is distributed on an
 "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 KIND, either express or implied.  See the License for the
 specific language governing permissions and limitations
 under the License.

  Copyright 2014 University Joseph Fourier, LIG Laboratory, ERODS Team

-->
<project
        xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>fr.liglab.erods.jmeter.mqtt</groupId>
    <artifactId>mqtt-jmeter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Apache JMeter :: MQTT Injector</name>
    <description>MQTT Injector for Apache JMeter</description>

    <properties>
        <jmeter-version>2.10</jmeter-version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.apache.jmeter</groupId>
            <artifactId>ApacheJMeter_core</artifactId>
            <version>${jmeter-version}</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.apache.jmeter</groupId>
            <artifactId>ApacheJMeter_java</artifactId>
            <version>${jmeter-version}</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>org.fusesource.mqtt-client</groupId>
            <artifactId>mqtt-client</artifactId>
            <version>1.4</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>${project.artifactId}</finalName>
        <defaultGoal>install</defaultGoal>
        <plugins>

    <plugin>
                <artifactId>maven-assembly-plugin</artifactId>

                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>

                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
~~~
