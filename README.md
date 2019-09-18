# SpringBootRedis
Template to implement Redis with SpringBoot

This page will walk through Spring Boot Data Redis example. Redis is an open source, in memory data-structure store that can be used as database, cache and message broker. Redis supports data-structure such as strings, hashes, lists, sets etc. Redis is a NoSQL storage and uses key/value to store data. Spring Boot provides spring-boot-starter-data-redis for Redis dependencies. Redis connections are obtained using LettuceConnectionFactory or JedisConnectionFactory. Lettuce and Jedis are Java Redis clients. Spring Boot 2.0 uses Lettuce by default. Spring Data provides RedisTemplate as the central class to interact with data in Redis store. To interact with string data, we can use string-focused extension StringRedisTemplate of RedisTemplate. Spring Data provides ListOperations, SetOperations, HashOperations etc to perform operations on Redis data and we can directly inject them in our Spring applications.

Technologies Used
Find the technologies being used in our example. 
1. Java 8 
2. Spring 5.0.7.RELEASE 
3. Spring Boot 2.1.18.RELEASE 
4. Maven 3.5.2 
5. IntelliJ

Maven File
Spring provides spring-boot-starter-data-redis to resolve Redis dependencies. It provides basic auto configurations for Lettuce and Jedis client libraries. By default Spring Boot 2.0 uses Lettuce. To get pooled connection factory we need to provide commons-pool2 dependency. Find the Maven file. 

$ cat pom.xml 
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.concretepage</groupId>
	<artifactId>spring-boot-app</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>spring-boot-app</name>
	<description>Spring Boot Application</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.3.RELEASE</version>
		<relativePath/>
	</parent>
	<properties>
		<java.version>9</java.version>
	</properties>
	<dependencies>
      <dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter</artifactId>
      </dependency>	
      <dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-starter-data-redis</artifactId>
      </dependency>		
	  <dependency>
	    <groupId>org.apache.commons</groupId>
	    <artifactId>commons-pool2</artifactId>
	  </dependency>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
      </dependency>
	</dependencies> 
	<build>
	  <plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
		</plugin>
	  </plugins>
	</build>
</project>

Using Lettuce Configurations
Spring Boot 2.0 starter spring-boot-starter-data-redis resolves Lettuce by default. Spring provides LettuceConnectionFactory to get connections. To get pooled connection factory we need to provide commons-pool2 on the classpath. To work with Lettuce we need following Maven dependencies.

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>		
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-pool2</artifactId>
</dependency> 

To configure Lettuce pool we need to use spring.redis.* prefix with Lettuce pool connection properties. Find the Lettuce pool sample configurations. 

application.properties
spring.redis.host=localhost 
spring.redis.port=6379
spring.redis.password= 

spring.redis.lettuce.pool.max-active=7 
spring.redis.lettuce.pool.max-idle=7
spring.redis.lettuce.pool.min-idle=2
spring.redis.lettuce.pool.max-wait=-1ms  
spring.redis.lettuce.shutdown-timeout=200ms 

We can override default Redis host, port and password configurations. Use max-wait a negative value if we want to block indefinitely.

Using Jedis Configurations
By default Spring Boot 2.0 starter spring-boot-starter-data-redis uses Lettuce. To use Jedis we need to exclude Lettuce dependency and include Jedis. Find the Maven dependencies to use Jedis.
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
  <exclusions>
    <exclusion>
	 <groupId>io.lettuce</groupId>
	 <artifactId>lettuce-core</artifactId>
    </exclusion>
  </exclusions>		    
</dependency>		
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
</dependency> 
jedis dependency will automatically resolve commons-pool2 on the classpath. 
To configure Jedis pool we need to use spring.redis.* prefix with Jedis pool connection properties. Find the Jedis pool sample configurations. 
application.properties
spring.redis.host=localhost 
spring.redis.port=6379
spring.redis.password= 

spring.redis.jedis.pool.max-active=7 
spring.redis.jedis.pool.max-idle=7
spring.redis.jedis.pool.min-idle=2
spring.redis.jedis.pool.max-wait=-1ms 
ListOperations
ListOperations is used for Redis list specific operations. Find some of its methods. 
leftPush(K key, V value): Prepends value to key. 
rightPush(K key, V value): Appends value to key. 
leftPop(K key): Removes and returns first element in list stored at key. 
rightPop(K key): Removes and returns last element in list stored at key. 
remove(K key, long count, Object value): Removes the first given number (count) of occurrences of value from the list stored at key. 
index(K key, long index): Fetches element at index from list at key. 
size(K key): Fetches the size of list stored at key. 

