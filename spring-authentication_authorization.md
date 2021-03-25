# Spring Autnetication
Spring provides authentication framework, the following is how to plug in the framework in the application.

## Add dependency
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
This automatically provides for security on all end points. To make changes to the defualt security provided by spring boot, we need to create a configurartion file that will permit/deny access to API URL specified by our RESTAPI application

```java
@Configuration //So that it picked up by the container for initialization
@EnableWebSecurity  //
public class WebSecurity extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();
        http.authorizeRequests().antMatchers("/user/**").permitAll();
    }
}

```
**NOTE** if you are using h2-console for development support ensure you pass the following in the configure overrident method

```java
http.headers().frameOptions().disable(); //Enable h2-console web URL
```
If you want to restrict request coming from a specific source. In case a API gateway is in use

```java
http.authorizeRequests().antMatchers("/**").hasIpAddress("192.168.0.1"); 
```
