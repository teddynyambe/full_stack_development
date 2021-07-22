# Spring Authentication
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

## Authentication
Spring security provides a default /login url that expects the following payload for authentication

```xml
{
  "username":"my-username",
  "password":"my-password"
}
```
Create a data model with similar fields

```java
public class LoginRequestModel {
    private String username;
    private String password;
    
    //Getters and setters
}
```
Create a class as an authentication filter, then this filter class will be registered with spring web security and the filter will be triggered each time a user sends a request to authenticate. And for this filter to perform authentication with username and password extend it with the UsernamePasswordAuthenticationFilter. Overright the attemptAuthentication method that will get the login request.

**Authentication Filter**
```java
public class AuthenticationFilter extends UsernamePasswordAuthenticationFilter {
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        try {
            LoginRequestModel cred = new ObjectMapper().readValue(request.getInputStream(), LoginRequestModel.class);
            return getAuthenticationManager().authenticate(
                    new UsernamePasswordAuthenticationToken(
                            cred.getUsername(),
                            cred.getPassword(),
                            new ArrayList<>()));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

    }
}
```
Now Register this authentication filter in the WebSecurity class automatically called by spring security to override its default operation

```java
@Configuration //So that it picked up by the container for initialization
@EnableWebSecurity  //
public class WebSecurity extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();
        http.authorizeRequests().antMatchers("/user/**").permitAll()
        .and()
        .addFilter(getAuthenticationFilter()); // REGISTER AUTHENTICATION FILTER HERE

        http.headers().frameOptions().disable(); //Enable h2-console web URL
    }

    private AuthenticationFilter getAuthenticationFilter() throws Exception { //CREATE AUTHENTICATION FILTER AND ASSIGN AUTHENTICATION MANAGER
        AuthenticationFilter authenticationFilter = new AuthenticationFilter();
        authenticationFilter.setAuthenticationManager(authenticationManager());
        return authenticationFilter;
    }
}
```

    Once the filter is registered and its collects the username and password from the request to authenticate spring security need to be told where to compare these login credentials with. This is achieved by overriding configure method of the Websecurity class that extends the WebSecurityConfiguererAdapter as follows:
    
    ```java
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userRegistrationService).passwordEncoder(bCryptPasswordEncoder);
    }
    ```
  The authentication manager builder specifies the user authentication scheme, in this case our custom UserRegistration class that needs to extend UserDtails of the springframework
  
  ```java
  public interface UserRegistrationService extends UserDetailsService {
    UserDTO createUser(NewUserDTO newUserDTO);
    UserDTO findByUsername(String username);
}
```

Then override the loadUserByUsername as follows:

```java
@Override
    public UserDetails loadUserByUsername(String username) throws FAPPUserNotFoundException {
        UserEntity userEntity = userRepository.findByUsername(username);
        if(userEntity == null) throw new FAPPUserNotFoundException(username);
        return new User(userEntity.getUsername(),userEntity.getEncryptedPasswoprd(), true, true, true, true, );
    }
  ```


