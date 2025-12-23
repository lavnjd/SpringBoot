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
    - Security context (for same browser auth , store auth object in session id)
 
    - ![WhatsApp Image 2025-12-23 at 7 08 00 PM](https://github.com/user-attachments/assets/93135a55-e2f4-40bf-94f9-f9dbb8789f87)
