# Spring MVC

## Spring MVC Overview
### What is Spring MVC?
- Framework for building web applications in Java
- Based on Model-View-Controller design pattern
- Leverages features of the Core Spring Framework(IoC,DI)

![image](https://user-images.githubusercontent.com/20329508/132282972-38f49cb0-1668-49db-a8d9-ea8661ac4a98.png)

### Spring MVC Benefits:
- The Spring way of building web app UIs in Java
- Leverage a set of reusable UI components
- Help manage application state for web requests
- Process form data: validation, conversion etc
- Flexible configuration for the view layer

<hr>

## Spring MVC Behind the Scenes

### Components of a Spring MVC Application
- A set of <strong>web pages</strong> to layout UI components
- A collection of Spring <strong>beans</strong> (controllers, services, etc...)
- Spring <strong>configuration</strong>(XML, Annotations or Java)

### How Spring MVC Works Behind the Scenes

#### Spring MVC Front Controller
![image](https://user-images.githubusercontent.com/20329508/132282972-38f49cb0-1668-49db-a8d9-ea8661ac4a98.png)

- Front controller known as DispatcherServlet
  - Part of the Spring Framework
  - Already developed by Spring Dev Team
- You will create
  - Model objects (orange)
  - View templates (dark green)
  - Controller classes (yellow)

#### Controller
- Code created by developer
- Contains your business logic
  - Handle request
  - Store/retrieve data (db, web services...)
  - Place data in model
- Send to appropriate view template

#### Model
- Model: contains your data
- Store/retrieve data via backend systems
  - database, web services, etc...
  - Use a Spring bean if you like
- Place your data in the model
  - Data can be any Java object/collection

#### View Template
- Spring MVC is flexible 
  - Supports many view templates
- Most common is JSP(Java Server Pages) + JSTL(JSP Standard Tag Library)
- Developer creates a page
  - Displays data  
- Other view templates supported
  - Thymeleaf, Groovy
  - Velocity, Freemarker, etc

<hr>

## Spring MVC Configuration

### Spring MVC Configuration Process

#### Add configurations to file: WEB-INF/web.xml
1. Configure Spring MVC Dispatcher Servlet
2. Set up URL mappings to Spring MVC Dispatcher Servlet

#### Add configurations to file WEB-INF/spring-mvc-demo-servlet.xml
3. Add support for Spring component scanning
4. Add support for conversion, formatting and validation
5. Configure Spring MVC View Resolver

##### Step 1: Configure Spring DispatcherServlet
```xml
<!-- File: web.xml -->
<web-app>
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>WEB-INF/spring-mvc-demo-servlet.xml</param-value>
    </init-param>

    <load-on-startup>1</load-on-startup>
  </servlet>
</web-app>
```

##### Step 2: Set up URL mappings to Spring MVC Dispatcher Servlet
```xml
<!-- File: web.xml -->
<web-app>
  <servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>WEB-INF/spring-mvc-demo-servlet.xml</param-value>
    </init-param>

    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

##### Step 3: Add support for Spring component scanning
```xml
<!-- File: spring-mvc-demo-servlet.xml -->
<beans>
  <!--  Step 3: Add support for component scannng  -->
  <context:component-scan base-package="com.luv2code.springdemo" />
</beans>
```

##### Step 4: Add support for conversion, formatting and validation
```xml
<!-- File: spring-mvc-demo-servlet.xml -->
<beans>
  <!--  Step 3: Add support for component scannng  -->
  <context:component-scan base-package="com.luv2code.springdemo" />
  
  <!--  Step 4: Add support for conversion, formatting and validation support  -->
  <mvc:annotaion-driven/>
</beans>
```

##### Step 5: Configure Spring MVC View Resolver
```xml
<!-- File: spring-mvc-demo-servlet.xml -->
<beans>
  <!--  Step 3: Add support for component scannng  -->
  <context:component-scan base-package="com.luv2code.springdemo" />
  
  <!--  Step 4: Add support for conversion, formatting and validation support  -->
  <mvc:annotaion-driven/>
  <!--    -->
  <bean 
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/view/" />
    <property name="suffix" value=".jsp" />
  </bean>
</beans>
```

#### View Resolver Configs:
- When your app provides a view name, Spring MVC will
  - prepend the prefix
  - append the suffix
```
/WEB-INF/view/show-student-list.jsp
-------------view name-------------
```

<hr>

## Developing Spring Controllers and Views

### Our first Spring MVC example

![image](https://user-images.githubusercontent.com/20329508/132293181-f3622687-6402-4d4d-9e45-6ce2ed6ace68.png)

### Development Process
1. Create Controller class
2. Define Controller method
3. Add Request Mapping to Controller method
4. Return View Name
5. Develop View Page

#### Step 1: Create Controller class
- Annotate class with @Controller
  - `@Controller inherits from @Component ... supports scanning
```java
@Controller
public class HomeController {
}
```

#### Step 2: Define Controller method
```java
@Controller
public class HomeController {

  public String showMyPage() {
  ...
  }
}
```

#### Step 3: Add Request Mapping to Controller method
```java
@Controller
public class HomeController {

  @RequestingMapping("/")
  public String showMyPage() {
   ... 
  }
}
```

#### Step 4: Return View Name
```java
@Controller
public class HomeController {

  @RequestingMapping("/")
  public String showMyPage() {
    return "main-menu";
  }
}
```
<strong>Finding the View Page</strong>
```xml
<bean 
    class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/view/" />
    <property name="suffix" value=".jsp" />
  </bean>
```
```
/WEB-INF/view/main-menu.jsp
<!-- View name: main-menu -->
```

#### Step 5: Develop View Page
```jsp
<!-- File: /WEB-INF/view/main-menu.jsp -->
<html>
  <body>
    <h2>Spring MVC Demo - Home Page</h2>
  </body>
</html>
```

<hr>

## Reading Form Data with Spring MVC

### High Level View:

![image](https://user-images.githubusercontent.com/20329508/132294865-612be018-0622-4083-a3c2-7846f7672cc9.png)

### Application Flow:

![image](https://user-images.githubusercontent.com/20329508/132295023-b26110f6-2865-4bf4-af44-6f2c40d6dbd3.png)

### Controller Class
```java
@Controller
public class HelloWorldController {
 
  // need a controller method to show the initial HTML form
  
  @RequestMapping("/showForm")
  public String showForm() {
    return "helloworld-form";
  }
  
  // need a controller method to process the HTML form
  
  @RequestMapping(".processForm")
  public String processForm() {
    return "helloworld";
  }
  
}
```

### Development Process
1. Create Controller class
2. Show HTML form
  - Create controller method to show HTML form
  - Create View Page for HTML form
3. Process HTML Form
  - Create controller method to process HTML Form
  - Development View Page for Confirmation
