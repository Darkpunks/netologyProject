__________________________________________________________________________
Домашнее задание к занятию "9.2 CI\CD"
__________________________________________________________________________

Знакомоство с SonarQube

__________________________________________________________________________

Подготовка к выполнению
__________________________________________________________________________

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.9.png">

__________________________________________________________________________
Основная часть

__________________________________________________________________________

Версия sonar-scanner 

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.1.jpg">


Первый запуск C ошибками: 

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.7.jpg">


смотрим ошибки 


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.6.jpg">

Исправлено 


<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.8.jpg">


__________________________________________________________________________
Знакомоство с Nexus
__________________________________________________________________________
Подготовка к выполнению
__________________________________________________________________________

Узнаем пароль от Nexus

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.5.jpg">
__________________________________________________________________________
Основная часть

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.4.jpg">


[maven-metadata.xml](https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/maven-metadata.xml)

```
<?xml version="1.0" encoding="UTF-8"?>
<metadata modelVersion="1.1.0">
  <groupId>netology</groupId>
  <artifactId>java</artifactId>
  <versioning>
    <latest>8_282</latest>
    <release>8_282</release>
    <versions>
      <version>8_102</version>
      <version>8_282</version>
    </versions>
    <lastUpdated>20220825191937</lastUpdated>
  </versioning>
</metadata>
```

__________________________________________________________________________

Знакомоство с Maven
__________________________________________________________________________

Подготовка к выполнению
__________________________________________________________________________
Проверяем mvn --version

<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/9.3.jpg">

__________________________________________________________________________

Основная часть
__________________________________________________________________________
Исправленный
[pom.xml](https://github.com/Darkpunks/netologyProject/blob/main/second%20part/9.2/pom.xml)

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.netology.app</groupId>
  <artifactId>simple-app</artifactId>
  <version>1.0-SNAPSHOT</version>
   <repositories>
    <repository>
      <id>maven-public</id>
      <name>maven-public</name>
      <url>http://localhost:8081/repository/maven-public/</url>
    </repository>
  </repositories>
  <dependencies>
    <dependency>
      <groupId>netology</groupId>
      <artifactId>java</artifactId>
      <version>8_282</version>
    </dependency> 
  </dependencies>
</project>
```




