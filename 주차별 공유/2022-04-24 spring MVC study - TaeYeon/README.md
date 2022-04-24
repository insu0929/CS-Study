# **springMVC의 구조**

![springMVC_구조](./image/springMVC_구조.png)

1. Handler Mapping 정보
    - String requestURI = request.getRequestURI();
    
    - return handlerMappingMap.get(requestURI);
        - handlerMappingMap :
            - /front-controller/v5/v1..
            
            - /front-controller/v5/v2..
            
              
    
2. HandlerAdapter 목록 조회
    - 핸들러 맵핑 정보를 바탕으로, 클래스에 대해 어떤 인터페이스로 구현했는지 확인을 위한 목록을 조회 하는것.
    
    - interface ControllerV4 또는 interface ControllerV5와 같은 것을 조회함
    
      
    
3. handle(Object handler)
    - 1번 정보와 대응하는 핸들러로 캐스팅을 하고
    
      
    
4. handler 호출
    - 해당 핸들러에 대한 process를 실행
    
      
    
5. ModelAndView 반환
    - 3번과 4번의 과정을 통해 ModelAndView에 대한 정보를 만들어서 반환
    
      
    
6. ViewResolver 호출
    - 논리적인 View에 대해서 물리적인 View로 변환하는 클래스를 호출
    
      
    
7. View 반환
    - 물리적인 주소를 가진 View를 반환
    
      
    
8. render(model)
    - 여러가지 정보를 가지고 화면은 렌더링 함
    



## **각 servlet 버전별 web.xml 스키마 헤더**

### 1. Servlet 4.0

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
version="4.0">
</web-app>
```

### 2. Servlet 3.1

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
version="3.1">
</web-app>
```

### 3. Servlet 3.0

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
version="3.0">
</web-app>
```

### 4. Servlet 2.5

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="servlet-2_5" version="2.5"
xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
</web-app>
```

### 5. Servlet 2.4

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="servlet-2_4" version="2.4"
xmlns="http://java.sun.com/xml/ns/j2ee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
</web-app>
```

### 6. Servlet 2.3

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">
<web-app>
</web-app>
```

### 7. Servlet 2.2

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.2//EN" "http://java.sun.com/j2ee/dtds/web-app_2_2.dtd">
<web-app>
</web-app>
```

- 2.4 부터는 DTD(Document Type Definition)를 사용하지 않고 XSD(XML Schema Definition)로 변경
- 3.1 부터는 서블릿을 sun에서 관리 안하는 듯.

# Spring MVC 설정 파일 (XML & Java Config)

## pom.xml 태그 설명 (maven에 대한 설정 파일)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
	http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion> // POM 모델 버전. 이것은 "4.0.0"라고 설정. 당분간이 버전이 바뀔 것은 없음.

	<groupId>tech-step01</groupId> // 그룹ID. 프로젝트 만들때 제작자와 회사, 단체 등을 식별하기 위함
	<artifactId>tech-step01</artifactId> // 아티팩트ID는 프로젝트 할당 고유ID
	<version>1.0.0</version> // 프로그램 버전
	<packaging>war</packaging> // 패키지 종류

	<!-- 사용할 저장소 등록 -->
	<repositories>
		<!-- 아시아나IDT 내부 리파지토리 연결 -->
		<repository>
			<id>asianaidt central</id>
			<url>https://***</url>
		</repository>
	</repositories>

	<!-- 사용할 버전 지정 -->
	<properties>
		<javax.servlet-version>4.0.1</javax.servlet-version> // 태그 이름은 지정 가능
		<javax.servlet.jsp-version>2.3.3</javax.servlet.jsp-version>
		<jstl-version>1.2</jstl-version>
		<org.springframework-version>5.3.19</org.springframework-version>
		<org.slf4j-version>1.7.36</org.slf4j-version>
		<ch.qos.logback-version>1.2.11</ch.qos.logback-version>
	</properties>

	<!-- 라이브러리 추가 -->
	<dependencies> // 의존 라이브러리 정보
		<!-- 서블릿 api -->
		<dependency> // 라이브러리의 구체적인 정보
			<groupId>javax.servlet</groupId> // 라이브러리 그룹ID
			<artifactId>javax.servlet-api</artifactId> // 라이브러리 아티팩트ID
			<version>${javax.servlet-version}</version> // 라이브러리 버전
			<scope>provided</scope> // 라이브러리가 이용되는 범위 지정
		</dependency>

		<!-- jsp api -->
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>javax.servlet.jsp-api</artifactId>
			<version>${javax.servlet.jsp-version}</version>
			<scope>provided</scope>
		</dependency>

		<!-- jstl -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl-version}</version>
		</dependency>

		<!-- spring webmvc -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- DB 관련 라이브러리 -->
		
		<!-- Lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<version>1.18.24</version>
			<scope>provided</scope>
		</dependency>
		
		<!-- 로깅 라이브러리 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.36</version>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.2.11</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>1.7.36</version>
		</dependency>

	</dependencies>

	<!-- config end -->
	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
				<configuration>
					<release>11</release>
				</configuration>
			</plugin>
			<plugin>
				<artifactId>maven-war-plugin</artifactId>
				<version>3.2.3</version>
			</plugin>
		</plugins>
	</build>
</project>
```

## 기본 설정 파일

- web.xml
    - WAS가 실행 되었을때 가장 먼저 실행되는 설정 파일
    - 특정 경로 요청에 대해 담당 디스패처 서블릿으로 전달
- dispatcher-servlet.xml
    - 서블릿에 대해서 정보를 기술하는 파일 → springMVC에서는 FrontController
    - controller, view resolver, interceptor 등의 빈 등록
- root-context.xml
    - 디스패처 서블릿 이전에 실행되는 공통에 대한 기본 설정 파일
    - service나 repository, datasource 등의 빈 등록

### web.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="
		http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
	id="WebApp_ID" version="4.0">

	<display-name>springMVC</display-name>

	<!-- 전체 공통 영역. 어플리케이션 공유영역 -->
	<!-- needed for ContextLoaderListener -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:/META-INF/config/root-context.xml</param-value>
	</context-param>

	<!-- Bootstraps the root web application context before servlet initialization -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- 스프링 설정 -->
	<!-- The front controller of this Spring Web application, responsible for handling all application requests -->
	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:/META-INF/config/dispatcher-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map all requests to the DispatcherServlet for handling -->
	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<!-- 파라미터 인코딩 필터 -->
	<filter>
		<filter-name>encodigFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>encodigFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
</web-app>
```

### dispatcher-servlet.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- @Controller가 붙은 클래스를 로딩 -->
	<context:component-scan base-package="com.example.springmvc" />
	
	<!-- 컨트롤러하고 url 매핑하고 연결시켜라. -->
	<mvc:annotation-driven />
	
	<!--  컨트롤러 업무를 처리하고 반화할 데이터 하고 화면을 지정 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix"	value="/WEB-INF/views/" />
		<property name="suffix" value=".jsp" />
	</bean>

	<!-- 정적리소스 처리. css, js image, audio -->
	
</beans>
```

### root-context.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<!-- DB 연결 -->
	
	<!-- mybtis. sqlSessionFactory -->
	
	<!--  mybatis-spring -->
	
	<!--  transactionManager -->
	
	<!-- 프로퍼티 관련설정 -->
	
	<!--  보안설정 -->
	
	<!--  연계 설정 -->

</beans>
```

## XML Config → Java Config (web.xml)

```java
public class WebXml implements WebApplicationInitializer {

	@Override
	public void onStartup(ServletContext servletContext) {

		AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext(); // Annotation 설정 파일의 경우
		// XmlWebApplicationContext context = new XmlWebApplicationContext(); XML 설정 파일의 경우
		
		context.setConfigLocation("com.asianaidt.academy.config"); // 설정 파일이 있는 위치 지정(패키지 위치)
		// context.setConfigLocation("com.asianaidt.academy.config.RootContext"); 직접 파일 지정 가능
		/* XML의 경우 */
		// context.setConfigLocation("classpath:/META-INF/config/*"); or
		// context.setConfigLocation("classpath:/META-INF/config/root-context.xml"); 직접 파일 지정 가능

		//		<!-- 공통 서블릿 전역 파라미터 -->
		//		<context-param>
		//			<param-name>contextConfigLocation</param-name>
		//			<param-value>classpath*:/**/context-*.xml</param-value>
		//		</context-param>
		//
		//		<!-- 서블릿 리스너 -->
		//		<listener>
		//			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		//		</listener>
		servletContext.addListener(new ContextLoaderListener(context));

		
		//		<!-- dispatcher 서블릿 설정 -->
		//		<servlet>
		//			<servlet-name>dispatcher</servlet-name>
		//			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		//			<init-param>
		//				<param-name>contextConfigLocation</param-name>
		//				<param-value>classpath:META-INF/dispatcher-servlet.xml</param-value>
		//			</init-param>
		//			<load-on-startup>1</load-on-startup>
		
		//		</servlet>
		//		<servlet-mapping>
		//			<servlet-name>dispatcher</servlet-name>
		//			<url-pattern>/</url-pattern>
		//		</servlet-mapping>
		ServletRegistration.Dynamic dispatcher = servletContext.addServlet("dispatcher", new DispatcherServlet(context));
		dispatcher.setLoadOnStartup(1);
		dispatcher.addMapping("/");

		//		<!-- encoding 필터 -->
		//		<filter>
		//			<filter-name>encodingFilter</filter-name>
		//			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		//			<init-param>
		//				<param-name>encoding</param-name>
		//				<param-value>UTF-8</param-value>
		//			</init-param>
		//		</filter>
		//		<filter-mapping>
		//			<filter-name>encodingFilter</filter-name>
		//			<url-pattern>/*</url-pattern>
		//		</filter-mapping>
		FilterRegistration.Dynamic encodingFilter = servletContext.addFilter("encodingFilter", new CharacterEncodingFilter());
		encodingFilter.setInitParameter("encoding", "UTF-8");
		encodingFilter.setInitParameter("forceEncoding", "true");
		encodingFilter.addMappingForUrlPatterns(null, true, "/*");

	}
}
```

## XML Config → Java Config (dispatcher-servlet.xml)

```java
@Configuration
@EnableWebMvc
// <context:component-scan base-package="com.asianaidt.academy" />
@ComponentScan("com.example.springmvc")
public class ServletContext implements WebMvcConfigurer {
	private static final String JSP_PATH="/WEB-INF/views/";
	private static final String JSP_EXT=".jsp";
	
	// 리졸버 등록
	//	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	//		<property name="prefix"	value="/WEB-INF/views/" />
	//		<property name="suffix" value=".jsp" />
	//	</bean>
	@Override
	public void configureViewResolvers(ViewResolverRegistry registry) {
		WebMvcConfigurer.super.configureViewResolvers(registry);
		registry.jsp(JSP_PATH, JSP_EXT);
	}
	
	
	// 정적 리소스 등록	
	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		// TODO Auto-generated method stub
		WebMvcConfigurer.super.addResourceHandlers(registry);
		registry.addResourceHandler("/**").addResourceLocations("/resources/");
	}

}
```

## XML Config → Java Config (root-context.xml)

기본적으로 작성된 내용이 없기 때문에 바껴도 설정 내용 없음

```java
@Configuration
public class RootContext {

}
```