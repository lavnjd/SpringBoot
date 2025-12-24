Spring Security

- Basic security will provide random password with user name as user. You can find the implementation in securityproperties.class file. To give custom user name and password you can change in application.properties
    - spring.security.user.name = "lavanya"
    - spring.security.user.password = "12434" Not a good security mechanism just basic one


- Spring securiy internal flow
    - Spring security filters checks whether the user is authenticated or not / there are numerious filter to identify different cases
    - Spring security filters will convert to http request object to Authentication object to make it accessible accross( username, password, isAuthenticated). For the first time isAuthenticated will be false
    - Filters will send the authentication object to authentication manager. and this manager will take care completion of authentication
    - Authentication manager will send Authentication providers (can be many) these will provide actual authentication by using userdetailsmanager(user details by username) and password encoder(for password comparison)
    - Authentication provider will let authentication manager whether isAuthenticated is true or false
    - Security context (for same browser auth , store auth object in session id) -> JSESSIONID
- Important core security methods
    - AuthorizationFilter(access denied or approved)
    - DefaultLoginPageGeneratingFilter(to create login page and doing filter)
    - AbstractAuthenticationProcessingFilter (security context with authentication object it has three filter-> usernamepassword, ott, webauth)
 
    - ![WhatsApp Image 2025-12-23 at 7 08 00 PM](https://github.com/user-attachments/assets/c17db872-f67e-42da-a89c-7615b1bf70c9)
 


Application - Banking

- Endpoints
  - Not Secure Endpoints
    - /contact : This service should accept the details from the contact us page in the UI and save to the DB
    - /notices: This service should send the notice details from the DB to 'NOTICES' page in the UI
  - Secure Endpoints
    - /myAccount: This should send the account details from db
    - /myBalance
    - /myLoans
    - /myCards
- How to customize certain APIs Secure and certain APIs public(SpringBootWebSecurityConfiguration)
  - @Bean
    SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests((requests) -> requests.anyRequest().authenticated());  ---> this is the line where all api endpoints are requied authentication, you can use permitAll(), denyAll()
        http.formLogin(withDefaults());
        http.httpBasic(withDefaults());
        return http.build();
    }
  - create a config to override the above funtionality
  -  @Bean
    SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests((requests) -> requests.requestMatchers("/myAccount", "/myCards", "/myLoans", "myBalance").authenticated()
                .requestMatchers("/notices", "/contacts", "/error").permitAll());
        http.formLogin(withDefaults());
        http.httpBasic(withDefaults());
        return http.build();
 
    }

- FormLogin and httpBasic
  - while opening an endpoint in browser you can disable the formlogin default by changing this line to http.formLogin(f -> f.disble());
  - In this case since the user is not enetering username and password it won't got to usernamepasswordauthenticationfilter instead it will got to basicauthorizationfilter
  - And the browser will give default login which will carry username and password as headers with AUTHORIZATION to basicauthorizationfilter
     -  header will have username:password with base64 encoding will be send to header
     -  to disbale httpBasic you can make http.httpBasic(hp -> hp.disable()) you will get 403 in browser
   
- For multiple User(UserDetailsManager)
  - JdbcUserDetailsManager : for user storage in DB
  - InMemoryUserDetailsManager - look into ProjectSecurityConfig this file in the project for more explanation
  

 

