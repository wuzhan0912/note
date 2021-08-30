

https://blog.csdn.net/zlgydx/article/details/51130627  这篇很好

Mirror of central , 仅仅作为central的mirror



```xml

<?xml version="1.0" encoding="UTF-8"?>

<!-- Licensed to the Apache Software Foundation (ASF) under one or more contributor 
	license agreements. See the NOTICE file distributed with this work for additional 
	information regarding copyright ownership. The ASF licenses this file to 
	you under the Apache License, Version 2.0 (the "License"); you may not use 
	this file except in compliance with the License. You may obtain a copy of 
	the License at http://www.apache.org/licenses/LICENSE-2.0 Unless required 
	by applicable law or agreed to in writing, software distributed under the 
	License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS 
	OF ANY KIND, either express or implied. See the License for the specific 
	language governing permissions and limitations under the License. -->

<!-- | This is the configuration file for Maven. It can be specified at two 
	levels: | | 1. User Level. This settings.xml file provides configuration 
	for a single user, | and is normally provided in ${user.home}/.m2/settings.xml. 
	| | NOTE: This location can be overridden with the CLI option: | | -s /path/to/user/settings.xml 
	| | 2. Global Level. This settings.xml file provides configuration for all 
	Maven | users on a machine (assuming they're all using the same Maven | installation). 
	It's normally provided in | ${maven.home}/conf/settings.xml. | | NOTE: This 
	location can be overridden with the CLI option: | | -gs /path/to/global/settings.xml 
	| | The sections in this sample file are intended to give you a running start 
	at | getting the most out of your Maven installation. Where appropriate, 
	the default | values (values used when the setting is not specified) are 
	provided. | | -->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
	<localRepository>/Users/harlan/.m2/repository</localRepository>
	<pluginGroups></pluginGroups>
	<proxies></proxies>
	<servers>  //配置用户名密码的区域
    <server>
    <id>nexus</id>
    <username>admin</username>
    <password>admin123</password>
    </server>
	<server>
    <id>localhostTomcat</id>
    <username>username</username>
    <password>password</password>
    </server>
	<server>
			<id>releases</id>
			<username>admin</username>
			<password>admin123</password>
		</server>
		<server>
			<id>snapshots</id>
			<username>admin</username>
			<password>admin123</password>
		</server>
		<server>
			<id>nexus3rd</id>
			<username>admin</username>
			<password>admin123</password>
		</server>
    </servers>
  
        <mirrors>  
			    <mirror>
		    <id>aliyunmaven</id>
		    <mirrorOf>central</mirrorOf>
		    <name>阿里云公共仓库</name>
		    <url>https://maven.aliyun.com/repository/public</url>
		</mirror>
  </mirrors>  

	<profiles>
		<profile>
			<id>nexus</id>
			<repositories>
				<repository>
					<id>nexus</id>
					<name>nexus</name>
					<url>http://maven.ipo.com/nexus/content/repositories/central/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</repository>
				<repository>
					<id>nexus3rd</id>
					<name>nexus3rd</name>
					<url>http://maven.ipo.com/nexus/content/repositories/thirdparty</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</repository>
				<repository>
					  <id>nexuspublic</id>
					  <name>nexuspublic</name>
					  <url>http://maven.ipo.com/nexus/content/groups/public/</url>
					  <releases>
						<enabled>true</enabled>
					  </releases>
					  <snapshots>
						<enabled>true</enabled>
					  </snapshots>
					</repository>
					<repository>
					  <id>nexussnap</id>
					  <name>nexussnap</name>
					  <url>http://maven.ipo.com/nexus/content/repositories/snapshots/</url>
					  <releases>
						<enabled>true</enabled>
					  </releases>
					  <snapshots>
						<enabled>true</enabled>
					  </snapshots>
					</repository>
        
        	<repository>
			      <id>central</id> // 可以轮流找，找到这里
			      <name>Central Repository</name>
			      <url>https://repo.maven.apache.org/maven2</url>
			      <layout>default</layout>
			       <releases>
						<enabled>true</enabled>
					  </releases>
					  <snapshots>
						<enabled>true</enabled>
					  </snapshots>
			    </repository>

			    	<repository>
					<id>extra</id>
					<url>http://maven.yonyoucloud.com/nexus/content/repositories/releases/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
				</repository>
			</repositories>
			<pluginRepositories>
				<pluginRepository>
					<id>nexus</id>
					<url>http://maven.ipo.com/nexus/content/repositories/central/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</pluginRepository>
			
			</pluginRepositories>
		</profile>
	</profiles>

	<activeProfiles>
		<activeProfile>nexus</activeProfile>   // java项目右上角点选
	</activeProfiles>
</settings>



```

