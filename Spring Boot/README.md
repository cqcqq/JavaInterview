**Spring Boot配置文件的加载优先级**
 - application.yml or application.properties
 - /project/config/
 - /project/
 - /project/src/main/resource/config/
 - /project/src/main/resource/
 - 优先级顺序从高到低，高优先级的配置会覆盖低优先级的配置。
 - https://docs.spring.io/spring-boot/docs/2.3.0.RELEASE/reference/html/spring-boot-features.html#boot-features-external-config



**Spring Boot自动配置**

- @SpringBootApplication

-   @EnableAutoConfiguration
	
	- ```java
	  @AutoConfigurationPackage
	  ```
	
	- ```java
	  @Import(AutoConfigurationPackages.Registrar.class)
	  public @interface AutoConfigurationPackage {
	  ```
	
	- ```java
	  @Import(EnableAutoConfigurationImportSelector.class)
	  public @interface EnableAutoConfiguration {
	  ```
	  
	- ```java
	  public class EnableAutoConfigurationImportSelector extends AutoConfigurationImportSelector
	  ```
	
	- ```java
	  public class AutoConfigurationImportSelector{
	      
	  	public String[] selectImports(AnnotationMetadata annotationMetadata) {
	  			List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
	  			configurations = filter(configurations, autoConfigurationMetadata);
	  			return configurations.toArray(new String[configurations.size()]);
	  		}
	  }
	  ```
	
	- ```java
	  public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
	  ```

总结：Spring Boot启动时，加载主配置类@SpringBootApplication，开启自动配置功能@EnableAutoConfiguration。

@AutoConfigurationPackage.Registrar把主配置类所在包及子包所有组件扫描注入到Spring容器中。

EnableAutoConfigurationImportSelector扫描并加载META-INF/spring.factories文件中需要的EnableAutoConfiguration自动配置类。



**自动配置报告**

 - 在application.yml配置

 ```yaml
   debug: true
 ```

```yml
=========================
AUTO-CONFIGURATION REPORT
=========================


Positive matches:
-----------------

   DispatcherServletAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.web.servlet.DispatcherServlet'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
      - @ConditionalOnWebApplication (required) found 'session' scope (OnWebApplicationCondition)

   DispatcherServletAutoConfiguration.DispatcherServletConfiguration matched:
      - @ConditionalOnClass found required class 'javax.servlet.ServletRegistration'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
      - Default DispatcherServlet did not find dispatcher servlet beans (DispatcherServletAutoConfiguration.DefaultDispatcherServletCondition)

   DispatcherServletAutoConfiguration.DispatcherServletRegistrationConfiguration matched:
      - @ConditionalOnClass found required class 'javax.servlet.ServletRegistration'; @ConditionalOnMissingClass did not find unwanted class (OnClassCondition)
      - DispatcherServlet Registration did not find servlet registration bean (DispatcherServletAutoConfiguration.DispatcherServletRegistrationCondition)

   DispatcherServletAutoConfiguration.DispatcherServletRegistrationConfiguration#dispatcherServletRegistration matched:
      - @ConditionalOnBean (names: dispatcherServlet; types: org.springframework.web.servlet.DispatcherServlet; SearchStrategy: all) found beans 'dispatcherServlet', 'dispatcherServlet' (OnBeanCondition)

Negative matches:
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'javax.jms.ConnectionFactory', 'org.apache.activemq.ActiveMQConnectionFactory' (OnClassCondition)

   AopAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required classes 'org.aspectj.lang.annotation.Aspect', 'org.aspectj.lang.reflect.Advice' (OnClassCondition)


Exclusions:
-----------

    None


Unconditional classes:
----------------------

    org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration

    org.springframework.boot.autoconfigure.web.WebClientAutoConfiguration

    org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration

    org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration
```
