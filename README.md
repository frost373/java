Obaseϵ����һ�׻���"spring+mysql+redis"��Java��˿�������, ����˵�Ǳ��߶��꿪���������.

* obase-webc: ����COC��MVC, ��web.xml����, �ܺ�֧��ǰ��˷���. �û�����ԭ��HttpServlet API����. 
* obase-mysql: һ������hibernate + mybatis��ORM���. �Զ���װ, һ��SQL������;: ��ֵ��ѯ, �����ѯ, ��ҳ��ѯ(֧���ֶ�����),  
* obase-jedis: ̸���Ͽ��, ���Ƕ�JedisPool��Դ��ȡ�ͷŵķ�װ.
* obase-config: ʵ��PropertySourcePlaceholderConfiguer�Ĺ���, ��֧��Redis, Mysql��̬���ö��ڸ��¹���. ����, ��֧���������AES128����, ����������������Ĵ��.
* obase-test: Ƕ��ʽTomcat8 + Junit4, ֧�ֻ��������Ķ�̬ע��. �������ײ���https��spring bean.
* obase-loader: �����ֽ��뷢��ʱ�õ�classloader. ������ҵ����Ƚ�ʵ��!

'''
�°汾,������,obase����0.x�汾���ɺ������ʽ����. 
- obase-web��spring-boot����������޷�Խ�, 
- obase-mysql֧�ֶ�̬sql, ��������hibernate��mybatis������! 
��Դ���ڹ�˾�ڲ����в���, ���ȶ����������������ĵ�!
'''

��Դobase����"spring+mysql+redis", ���˼·������չ������... ��memcache, postsql. �ڴ˾Ͳ���������. 

# obase-webc
* obase-webc���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-webc</artifactId>
	<version>0.8.2</version>
</dependency>
```
## obase-webc��ʲô?
obase-webc�ǻ���servlet 3.0+��AsyncContextʵ�ֵ���web.xml����ģʽ.��Filter����ʵ����Spring MVC�Ĺ���, ���Ƴ���HandlerMapping��ViewResolver, ��COC��Spring MVC�ķ�������. �ŵ���ʲô? ������.

## obaes-webc��ô��?

obase-webc��ʹ�÷���:

Դ��mavenĿ¼�ṹ�ο�: https://github.com/obase/java/tree/master/obase-demo, �û���Ҫ�̳�obase-parent
```
<parent>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-parent</artifactId>
	<version>1.1.0</version>
</parent>
```
���涨����spring, servlet, jsp�ĺ��İ汾.

+ ��1��: ����/META-INF/webc.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<webc xmlns="http://obase.github.io/schema/webc" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://obase.github.io/schema/webc
	https://obase.github.io/schema/obase-webc-1.0.xsd">

</webc>
```

/WEB-INF/webc.xml��/META-INF/webc.xml��obase-webc����"����".  

+ ��2��: ����/META-INF/servletContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">

	<context:component-scan base-package="com.github.obase.demo.controller" />

</beans>
```

+ ��3��: ����Controller
```
package com.github.obase.demo.controller;

import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;

import com.github.obase.webc.Kits;
import com.github.obase.webc.annotation.ServletMethod;

@Controller
public class TestController {

	@ServletMethod
	public void hello(HttpServletRequest request, HttpServletResponse response) throws IOException {
		String name = Kits.readParam(request, "name");
		Kits.writeSuccessMessage(response, "hello " + name);
	}

}

```

+ ��4��: ����HttpServer
```
package com.github.obase.test;

public class HttpServer {

	public static void main(String[] args) {
		EmbedTomcat.start();
	}

}

```

����HttpServer, ���������"http://localhost/test/hello?name=jason.he"
```
{"errno":0,"data":"hello jason.he"}
```

## obase-webc��spring-boot����

- maven����
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.github.obase.boot</groupId>
	<artifactId>obase-boot-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<properties>
		<spring.boot.version>1.5.7.RELEASE</spring.boot.version>
		<spring.version>4.3.10.RELEASE</spring.version>
		<jackson.version>2.9.0</jackson.version>
		<java.version>1.8</java.version>
		<obase.version>1.1.0-SNAPSHOT</obase.version>
	</properties>

	<parent>
		<groupId>com.github.obase</groupId>
		<artifactId>obase-parent</artifactId>
		<version>1.1.0</version>
	</parent>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-dependencies</artifactId>
				<version>${spring.boot.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<dependencies>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>javax.servlet.jsp-api</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>com.github.obase</groupId>
			<artifactId>obase-webc</artifactId>
			<version>${obase.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>

	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>${spring.boot.version}</version>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```
����properties��ֵȷ��obase-webc��spring-bootʹ����ͬ�汾����.

- java����

```
package test.main;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletContextInitializer;

import com.github.obase.webc.WebcServletContainerInitializer;

@SpringBootApplication
public class AppMain extends WebcServletContainerInitializer implements ServletContextInitializer {

	public static void main(String[] args) {
		SpringApplication.run(AppMain.class, args);
	}

}

```
ִ��mvn clean package����ɵõ�ִ�е�war.

obase-webc�ĳ���: ����spring-webmvc, ��spring-web + Servlet 3.0+ �������COC����һ��ȫ�µ�MVC. 

+ ǰ��˷���, ʵ����web.xml����, ��src/main/webappĿ¼��ȫ����ǰ���Ŷ�.
+ ͳһURLӳ��, ����COC��ӳ�����.
+ �ɼ�ɷ�, ��ʱ����һ��webc.xml����ʹ��, ����������web.xml, Ҳ���������ø��ֿ��servlet;  ����ʱ����ϸ���ȿ���API���ʲ���, �ӹ�Spring Security�Ĺ���. 
+ ��servlet����, ����������spring-mvc�޷�����(��ʷ����ԭ��). --- ����Filter����ʵ��.
+ ֧�ֶ�̬session cookieУ��. --- ���ú�cookie�Դ�ʱ�����hashָ��.
+ ֧������restful��API, ֧��UI��API������ͬ��Controller. --- @Controller + @ServletMethod
+ ֧��΢������. --- @Service + @InvokerService
+ ֧�ֶ������+�ֲ�ʽ�Ự.
+ û�з������. reflect̫out��, ����ASM + ServletMethodFilter �Զ���װ���ش��뵽@ServletMethod����.
+ ������С����. �õ���ؼ�, Ҳ������Ҫ. ʹ����ֻ��֪��@InvokerService, @ServletMethod, SerlvetMethodFilter, ServletMethodProcssor����ע���API, ��������HttpServlet API, ����Spring API.

��������һһ����.

# obase-jedis
* obase-jedis ���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-jedis</artifactId>
	<version>0.8.2</version>
</dependen
```

# obase-test

* obase-test ���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-test</artifactId>
	<version>0.8.2</version>
</dependen
```
## obase-test��ʲô?
��װ��embedded tomcat 8��junit4, �򵥼���ʵ��https�Ĳ���, �Լ�Spring Context�����ĵ�Ԫ����. Ϊʲô����Jetty9? �ù����֪��jetty9 ����Servlet 3.0+��֧���жෳ!

# obase-config

* obase-config ���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-config</artifactId>
	<version>0.8.2</version>
</dependen
```
## obase-config��ʲô?
����Spring, ʵ��PropertySourceConfigurer�Ĺ���, �����... ֧��redis��mysql�Ķ�̬���û�ȡ�붨ʱ����, ����... ����������, �����������������˵��һ�ּ��Ĵ���ʽ.

# obase-loader

* obase-loader ���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-loader</artifactId>
	<version>0.8.2</version>
</dependen
```
## obase-loader��ʲô?
���jar��Ҫ���ܷ���ô? obase-loader����spring context��classloader����, ʵ�ּ���ʱ�����ֽ��빦��.

# obase-mysql

* obase-mysql���°汾
```xml
<dependency>
	<groupId>com.github.obase</groupId>
	<artifactId>obase-mysql</artifactId>
	<version>0.8.2</version>
</dependency>
```

## obase-mysql��ʲô?
����˼��, obase-mysql�����mysql��һ��jdbc��װ����.��ʵ����Ŀʹ���˼����hibernate, mybatis, spring-jdbc����ÿ�ֿ�ܸ����ص�,ͬʱ����һЩ��������ĵط�:

����Hibernate: 
* Hibernate��װEntity(����hibernate.hbm2ddl.auto�����Զ������ṹ),�����������д�κ�SQL,����ˬ. ����������? Hibernate��HQL̫����,��֧���Ӳ�ѯ,������(left join, right join)��ѯ��������@OneToMany, @ManyToMany �ȹ���ע��. �ܶ�ͬѧ��˵Hibernate֧��Native SQLѽ. ��,���ڱ���SQL�ӿڿ���д�κ�SQL,����Ҫ�󿪷��Լ�ȥ��װObject[]���. ����������龰����, Ϊʲô������ȥ��Spring-jdbc��RowMapper��? �������ֻᱧԹ, ��ÿ�ű���ҪȥдһЩ��ͬ��CRUD SQL��!!!
* Hibernate��HQL����antlr�﷨������ADT(�����﷨��),�����Dialect����ת�ɾ������ݿ��SQL�﷨, ����ܶ�ϲ���ķ�ҳ��ѯ, ��mysqlת��limit clause, ��oracle�����rownum���й���. ���м侭��2��ת��, ����ô? һֱ�Ǻܶ࿪����Թ�Ľ���.
* Hibernate�ṩ��һ������,������������ö�����������ݿ��ѯƵ��,������ݲ�ѯ����.ĳ�ֲ���,����cache����ĸ߳�����ʵ������������Ŀ,̾Ϊ��ֹ. ����...����...���ڴ�����ݵĻ�����Ӧ��Ϊ��HA(�߿���)��LB(���ؾ���), ������ö������ģʽ. �����������, ��ɫ�����Ļ�����������㷢���ٷ���, ������CAP���۵õ�����ʵ����֤. ���,Ϊ���ƹ�����,ÿ�β������������flush(), clear()...�α���? ����mybatis, spring-jdbc���Ǹ�ʡ��ô? ����...���������������"����"��.
* Hibernate�������,��3��ʹ�ò��, ѧϰ�Ѷ���㷭��: 
    1. ��ӳ��, �������ֶ�ӳ��,���ֶ�ӳ��. �����������ͣ��������׶�.
    2. �����, ����һ��һ����,һ�Զ����,��Զ����. �����Բ�ѯ���ܵ�Ӱ��ܴ�, �Ż����Ծ��ǽ���������־�������lazy,����eager,��Ҫʮ��С��"����"��LazyInitializationException. ����, Hibernate�ٷ��ر�ǿ�������������,���鲻Ҫ����3��. ����, �Ƽ���С����Ϊ����������...�ȵ�. ��Щû��Hibernate����ϵͳ�о��Ŀ���, ��ֻ�ܽ������ޱ�Ҫ,���������Ŀ��ʹ�ñ����.
    3. ��̳�, Hibernate�ļ̳в�����3��ʵ�ֲ���: ����̳в���(table per class), ���Joined����(table per subclass), �ͱ�ʶ�ֶβ���(table per class). ѡ��ͬ���Ա������, Ӱ���������Ĳ��ǳ���, ���Ǳ�ṹ�����ݴ洢. �Ͼ�����Ķ���������д, ����������, �㻹��"����"�����赭д˵һ��: "����������������"ô? ����Ӣ��! 

�������Ҵ�2009�꿪ʼʹ��Hibernate������һЩ��ʵ����! ���ܽ�: ֻ��hibernate�ı�ӳ��, ����ʹ�ñ����,���Ҳ㼶<2. �������ñ�̳�...����,�߲������ܵĲ�ѯ�ӿ�ʹ��Native SQL. ��Ҫʱʹ��procedure.

����Mybatis(ibatis):
* mybatis�ĺ��ľ���SqlMap, ����, ����׸��, ���mybatis����������? 
* mybatis�Ķ�̬SQL��ǩ, �������ǹ��ܹ�ǿ��! ���ڶ�̬ƴ��,ֻ��˵��"ɽ��ˮ�Ĺ�ϵ", ������ɽ, ������ˮ. ��̬������������Щϲ��ƴ��SQL�Ŀ�������, �ر���ʹ��$�����ľ�̬�滻. ֻҪ���ڶ�̬ƴ�Ӳ���, ����SQLע��ķ���! 
* mybatis֧��SQL�����������Զ�ӳ��,������hibenrate���ܶ�. ������ʵ���ǻ��ڷ����,ֱ�ӵĽ�����ǵ���mybatis��hibernate native sqlû��̫�����������.

���������⼸��ʹ��mybatis������һЩ���, ��ĳ�ֲ�����˵, mybatis�ܹ����ϴ������������! �ѹ��Ա�ϵ�Ļ��������о���ibatis. ����mybatisû�����������Ľӿ�, �Լ�������hibernate�����Զ��������ݱ�ṹ, ������ҳҲ��Ҫ�������������...������Щʧ��.

�ܶ���ĿͬʱӦ����hibernate��mybatis,ȡ������,����˼·�ܲ���! ���Ƕ�������һ������������ʵ: hibernate��mybatis��spring����ļ��ɽӿڲ�ͬ! ����֮, ��Ŀ����Ĵ���, Ҫô��hibernate, Ҫô��mybatis. ���ö���, �����ɺܴ������. 

��������������ô��, ���Ӧ������spring-mysqlclient����Ƴ����˰�. ˵���˾����ۺ���hibernate��mybatis�ĺ�������, ͬʱʹ��ASM�ֽ��뼼�������̬����, ������ѯ���̵�����.

## obase-mysql����Щ����

* mysqlclient����updateTable����, �����Զ�����@Table��������ݱ�ṹ. ��������"����"����. �������:
    1. ���������, ���Զ�������, �Լ���������, ���, ����.
    2. ��������, �����ṹ:
        1. �Ƚ��ֶ�, �������ͬ���ֶ�, �����޸�.
        1. �Ƚ�����, ����������������޸�. �����������ֶ��Ƿ���ͬ, ����ʾ��ؾ�����Ϣ.
        1. �Ƚ����, �������ͬ������, �����޸�.
        1. �Ƚ�����, �������ͬ������, �����޸�.
        
        ��ϸ����, ���Բ鿴 **com.github.risedragon.mysql.jdbc.SqlDdlKit.processUpdateTable()** ����.
        
* mysqlclient�ṩ�����¼��insert, update, replace, merge, delete, batchInsert, batchUpdate, batchReplace, batchMerge, batchDelete, select, select2, selectFirst, selectRange, selectPage����. ��Щ����ֻ������@Table, @Columnע�⼴��, ����д�κ�SQL. ��ϸ�÷�, ���Բμ�<��������>.

* mysqlclient�ṩ����SQL֧��, ���Բ�����������ȡ�Զ���װ. ����Ԥ�����SqlType��JavaType��Ԥ����scalar����, �û����Ի���ActionMeta�ӿڶ���ʵ��, ͨ�� **JdbcAction.markSqlType()** ע�ᵽ���, �������֧�ָ��ϲ���IN(:list). ��ϸ�÷�, ���Բμ�<��������>.

* mysqlclient�ṩSQL�ĳ�����ѯ�ӿ�: query, queryFist, queryRange, queryPage. ���з�ҳ�ӿ�Page, ���ṩ�ֶ�������. ��ϸ�÷�, ���Բμ�<��������>.

* mysqlclient�ṩSQL�ĳ��������ӿ�: execute, batchExecute. 

* ����, mysqlclient�ṩ�����������ӿ�MysqlClientExt���㲻��ҪSpring��PlatformTransactionManager���������Ӧ��, ��ȫ���ϸ���ȿ���������ύ��ع�. ��ϸ�÷�, ���Բμ�<��������>

## obase-mysql��������

* ����ʵ��
   
ʹ��@Tableע��
```java
    @OptimisticLock(column = "version")
public abstract class Base {

	@Column(key = true, autoIncrement = true, comment = "��������")
	Long id;

	@Column(length = 16)
	String createBy;

	@Column
	Date createTime;

	@Column(length = 16)
	String modifyBy;

	@Column
	Date modifyTime;

	@Column
	Long version;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getCreateBy() {
		return createBy;
	}

	public void setCreateBy(String createBy) {
		this.createBy = createBy;
	}

	public Date getCreateTime() {
		return createTime;
	}

	public void setCreateTime(Date createTime) {
		this.createTime = createTime;
	}

	public String getModifyBy() {
		return modifyBy;
	}

	public void setModifyBy(String modifyBy) {
		this.modifyBy = modifyBy;
	}

	public Date getModifyTime() {
		return modifyTime;
	}

	public void setModifyTime(Date modifyTime) {
		this.modifyTime = modifyTime;
	}

	public Long getVersion() {
		return version;
	}

	public void setVersion(Long version) {
		this.version = version;
	}

}

@Table(engine=Engine.InnoDB, characterSet="UTF8")
public class Employee extends Base {

	@Column(length = 64, comment = "����")
	String cardNo;
	@Column(length = 16, comment = "����")
	String type;
	@Column(length = 16, comment = "����")
	String name;
	@Column(length = 8, comment = "�Ա�")
	String gender;
	@Column(length = 16, comment = "����")
	String groupName;
	@Column(length = 16, comment = "�ֻ�����")
	String phone;
	@Column(length = 18, comment = "���֤����", unique = true)
	String sid;
	@Column(length = 18, comment = "���պ���", unique = true)
	String passportNo;
	@Column(length = 18, comment = "���պ���ƴ��")
	String passportAbbr;
	@Column(length = 8, comment = "���ⷿ���")
	String room;
	@Column(comment = "��н���μ���")
	Date paidHoliday;
	@Column(length = 8, defaultValue = "���", comment = "�ֿۼ���")
	String holidayType;
	@Column(length = 18, comment = "�칫�ص�")
	String officeLocation;

	@Column
	BigDecimal other;

	public String getCardNo() {
		return cardNo;
	}

	public void setCardNo(String cardNo) {
		this.cardNo = cardNo;
	}

	public String getType() {
		return type;
	}

	public void setType(String type) {
		this.type = type;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getGender() {
		return gender;
	}

	public void setGender(String gender) {
		this.gender = gender;
	}

	public String getGroupName() {
		return groupName;
	}

	public void setGroupName(String groupName) {
		this.groupName = groupName;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getSid() {
		return sid;
	}

	public void setSid(String sid) {
		this.sid = sid;
	}

	public String getPassportNo() {
		return passportNo;
	}

	public void setPassportNo(String passportNo) {
		this.passportNo = passportNo;
	}

	public String getPassportAbbr() {
		return passportAbbr;
	}

	public void setPassportAbbr(String passportAbbr) {
		this.passportAbbr = passportAbbr;
	}

	public String getRoom() {
		return room;
	}

	public void setRoom(String room) {
		this.room = room;
	}

	public Date getPaidHoliday() {
		return paidHoliday;
	}

	public void setPaidHoliday(Date paidHoliday) {
		this.paidHoliday = paidHoliday;
	}

	public String getHolidayType() {
		return holidayType;
	}

	public void setHolidayType(String holidayType) {
		this.holidayType = holidayType;
	}

	public String getOfficeLocation() {
		return officeLocation;
	}

	public void setOfficeLocation(String officeLocation) {
		this.officeLocation = officeLocation;
	}

	public Long getVersion() {
		return version;
	}

	public void setVersion(Long version) {
		this.version = version;
	}

	public BigDecimal getOther() {
		return other;
	}

	public void setOther(BigDecimal other) {
		this.other = other;
	}

	public String toString() {
		return JsonUtils.writeValueAsString(this);
	}
}

```

ʹ��\<table\>��ǩ
```xml
<?xml version="1.0" encoding="UTF-8"?>
<mysql>
    <table>com.yy.risedev.myweb.entity.Employee</table>
    ...
</mysql>
```
* ����־�����ʱ��JdbcAction
    
    JdbcAction�ӿ�ʵ��SQL��������������ȡ�ķ�װ. mysqlclient����ASM�Զ�������صĴ�������. 
    
    **ע��: @Tableע���<table>��ǩ�����ʵ���Ѿ��Ǹ�Meta, �������ظ�����!**

ʹ��@Metaע��
```java
@Meta
public class EmpPart {
	Long id;
	Long version;
	String cardNo;
	String groupName;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public Long getVersion() {
		return version;
	}

	public void setVersion(Long version) {
		this.version = version;
	}

	public String getCardNo() {
		return cardNo;
	}

	public void setCardNo(String cardNo) {
		this.cardNo = cardNo;
	}

	public String getGroupName() {
		return groupName;
	}

	public void setGroupName(String groupName) {
		this.groupName = groupName;
	}

	public String toString() {
		return JsonUtils.writeValueAsString(this);
	}
}
```

ʹ��\<meta\>��ǩ
```xml
<?xml version="1.0" encoding="UTF-8"?>
<mysql>
    <table>com.yy.risedev.myweb.model.EmpPart</table>
    ...
</mysql>
```

* ����sql
ʹ��\<sql\>��ǩ

 **ע��:namespace�ǿ�ѡ��,һ������,ʹ��SQLʱ�������,��������xml��Ϊtest.insertPartEmployee**
[����schema����](https://github.com/risedragon/schema/blob/master/risedev-mysql-1.0.xsd)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mysql namespace="test">
    <sql id="insertPartEmployee">
	<![CDATA[INSERT INTO Employee (id,version,cardNo,groupName) VALUES(:id,:version,:cardNo,:groupName) ON DUPLICATE KEY UPDATE version=version+1, cardNo=:cardNo, groupName=:groupName]]>
    </sql>
    ...
</mysql>
```

* ��spring������mysqlclient

    �������������ʵ�� **MysqlClientPlatformTransactionImpl**
    ```xml
    	<bean id="mysqlClient" class="com.github.risedragon.mysql.impl.MysqlClientPlatformTransactionImpl" init-method="init">
		<property name="dataSource" ref="dataSource" />
		<property name="packagesToScan" value="com.yy.risedev.myweb.entity,com.yy.risedev.myweb.model" />
		<property name="configLocations" value="classpath:config/*.xml" />
		<property name="showSql" value="true" />
		<property name="updateTable" value="true" />
	</bean>

    ```
    ��̹��������ʵ�� **MysqlClientConnectTransactionImpl**
    ```xml
    	<bean id="mysqlClient" class="com.github.risedragon.mysql.impl.MysqlClientConnectTransactionImpl" init-method="init">
		<property name="dataSource" ref="dataSource" />
		<property name="packagesToScan" value="com.yy.risedev.myweb.entity,com.yy.risedev.myweb.model" />
		<property name="configLocations" value="classpath:config/*.xml" />
		<property name="showSql" value="true" />
		<property name="updateTable" value="true" />
	</bean>

    ```   
    ��������˵����
    

    ���� | ���� | Ĭ��ֵ
    ---|---|---
    dataSource | ����Դ���ã��κ�java.sql.DataSourceʵ�� | ��
    packagesToScan | ɨ��@Table��@Meta�����ʼ������ֵ�ö��ŷָ�������"a.b.c,a.b.d" | ��
    configLocations | ����sql xml��Spring Resource Pattern, ��ֵ�ö��ŷָ������硰classpath:a/b/c/\*.xml,classpath:a/b/d/\*.xml�� | ��
    showSql | ��ʾ������SQL. ������Ի����򿪣����������ر� | false, Ĭ�Ϲر�
    updateTable | �Ƿ���±�ṹ. ���Ϊtrue, �����@Table��@Column�Ķ�����±�ṹ. ��ϸ����μ�<mysqlclient����updateTable����>. | false, Ĭ�Ϲرմ�����!

    
* ��spring��֧����������

    ����ע��@Transactional
    ```xml
    <bean id="transactionManager" class="com.github.risedragon.spring.transaction.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	<tx:annotation-driven transaction-manager="transactionManager" />
    ```
    ����TransactionTemplate
    ```xml
    <bean id="transactionManager" class="com.github.risedragon.spring.transaction.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	<bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
		<constructor-arg ref="transactionManager"/>
	</bean>
    ```

* ʵ�����: insert, insertIgnore, replace, update, merge, delete, select, select2, batchInsert, batchInsertIgnore, batchReplace, batchUpdate, batchMerge, batchDelete, selectFirst, selectRange, selectPage��
```java
@Service
@Transactional
public class GenericService {

	@Autowired
	MysqlClient mysqlClient;

	public void insret() throws SQLException {
		Employee emp = new Employee();
		emp.setGroupName("���Բ���");
		emp.setCardNo("135-137");
		Long id = mysqlClient.insert(emp, Long.class);

		System.out.println(id);
		throw new SQLException();
	}

	public void update() throws SQLException {
		Employee emp = mysqlClient.selectByKey(Employee.class, 3);
		emp.setPaidHoliday(new Date());

		System.out.println(mysqlClient.update(emp));
	}

	public void replace() throws SQLException {
		Employee emp = new Employee();
		emp.setId(4L);
		mysqlClient.select2(emp);
		emp.setCardNo("111-222-333");
		emp.setPaidHoliday(new Date());
		System.out.println(mysqlClient.replace(emp));
	}

	public void merge() throws SQLException {
		Employee emp = new Employee();
		emp.setCardNo("999-666-333");
		emp.setPaidHoliday(new Date());
		System.out.println(mysqlClient.merge(emp, BigDecimal.class));
	}

	public void delete() throws SQLException {
		Employee emp = mysqlClient.selectByKey(Employee.class, 5L);
		System.out.println(emp);
		System.out.println(mysqlClient.deleteByKey(Employee.class, 6L));
	}

	public void selectPage() throws SQLException {
		Page<Employee> page = new Page<>(2, 0, "id", true);
		mysqlClient.selectPage(Employee.class, page);
		System.out.format("total=%d,data=%s", page.getTotal(), page.getData());

	}

	public void showTables() throws SQLException {
		List<Object> list = mysqlClient.query("test.showTables", null, null);
		System.out.println(list);
	}

	public void selectBySql() throws SQLException {
		// List<EmpPart> list = mysqlClient.queryRange("test.selectPartEmployee", EmpPart.class, 0, 2, Arrays.asList(5, "%666%"));
		long start = System.currentTimeMillis();
		for (int i = 0; i < 100000; i++) {
			Page<EmpPart> page = new Page<>(0, 0, "id", true);
			mysqlClient.queryPage("test.selectPartEmployee", EmpPart.class, page, Arrays.asList(5, "%"));
		}
		long end = System.currentTimeMillis();
		System.out.println("used time:" + (end - start));
		// System.out.println(JsonUtils.writeValueAsString(page));
	}

	public void insertBySql() throws SQLException {

		long start = System.currentTimeMillis();
		int base = 1000;
		EmpPart[] array = new EmpPart[1000];
		for (int i = 0; i < 1000; i++) {
			EmpPart part = new EmpPart();
			part.setCardNo(String.format("000-001-%05d", i + 2000));
			part.setId(i * 1L + base);
			part.setGroupName("����SQL����");
			part.setVersion(null);
			array[i] = part;
		}
		Long[] result = mysqlClient.batchExecute("test.insertPartEmployee", array, Long.class);
		long end = System.currentTimeMillis();
		System.out.println("used time:" + (end - start));
		System.out.println(Arrays.asList(result));
	}

	public void executeCallback() throws SQLException {
		mysqlClient.callback(new ConnectionCallback<Void>() {

			@Override
			public Void doInConnection(Connection conn) throws SQLException {
				for (int i = 0; i < 10000 * 10; i++) {
					Statement stmt = conn.prepareStatement("show tables");
				}
				return null;
			}
		});
	}

	public void insertIgnore() throws SQLException {
		long start = System.currentTimeMillis();
		List<Employee> list = new LinkedList<Employee>();
		for (int i = 0; i < 10000; i++) {
			Employee emp = new Employee();
			emp.setId(i * 1L + 1);
			emp.setCardNo(String.format("000-001-%05d", i + 3000));
			emp.setGroupName("group " + i);
			emp.setOther(BigDecimal.valueOf(i));
			mysqlClient.merge(emp, Long.class);
		}
		// Long[] result = mysqlClient.batchInsertIgnore(list.toArray(), Long.class);
		long end = System.currentTimeMillis();
		System.out.println("used time:" + (end - start));
		// System.out.println(Arrays.toString(result));
	}

	public void selectRange() throws SQLException {
		List<Employee> list = mysqlClient.selectRange(Employee.class, 0, 1000);
		for (Employee item : list) {
			System.out.println(item);
		}
	}
}
```

* SQL����: query, queryFirst, queryRange, queryPage, execute, batchExecute

    <����

## obase-mysql �߼�Ӧ��

* �����ֹ�������

* ���JAVA�̳���ϵ��ʵ���������ݲ����������

* ֧�����������ݷ��ؽӿ�

##  obase-mysql ������չ

* ��չJdbcAction
    
    ����Ҫ��EmpPart�Զ���JdbcActionʵ��, ֻ��Ҫ���������淶<targetClass>$JdbcActionʵ�����༴��. ����
```java
    public class EmpPart$JdbcAction extends JdbcAction{
      ...
    }
```
    ͨ��AsmKit.newJdbcAction()�Ϳ��Լ��ش���ʵ��.

* ��չActionMeta
    
    �����ĳ���ֶ�����Ҫ�ر���, ��ʵ��ActionMeta, ����JdbcAction.markSqlType()ע��, �������������͵��ֶκ���Զ����ø�ActionMeta���ò�������ȡ���. ���缯�ϲ����Ĵ���.


# ��ϵ��ʽ
    
������ | ��ϵ��ʽ
---|---
jasonhe | jasonhe.hzw@foxmail.com, QQ:1255422783
