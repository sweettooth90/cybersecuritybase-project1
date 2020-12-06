# CYBER SECURITY BASE - Course Project I

## Setup

##### How to setup:
1. Clone project and run with ./gradlew run
2. Open a web browser and go to localhost:8080

It is recommended to use Java 8.

## Issues

### FLAW 1

#### 3. Sensitive Data Exposure

##### Description:
Re-entering the same name on form page will cause a information leak where sensitive information will become visible. One of the reasons for this is the lack of authorization. Also when data is stored in a database a some kind of encryption should be executed.

##### How to reproduce:
1. Start the server
2. Go to localhost:8080
3. Sign in (username: "ted", password: "ted")
4. When on form page enter "ted" as a name and "address" as an address
5. Press submit and receive confirmation message about successful sign in for the event
6. Navigate to localhost:8080/form
7. Enter "ted" as a name and press submit
8. When the message is received, Ted's address will be visible

##### How to fix:
To fix the issue a HttpSession is needed. SingupController.java have methods called defaultMapping() (row 20) and loadForm() (row 25). A "httpSession.invalidate()" needs to be added into beginning of those methods to prevent the issue.


### FLAW 2

#### 7. Cross-Site Scripting XSS

##### Description:
Any kind of script which is entered in to the input field is executed when rendering the page. This is caused by lack of input sanitization and can lead to unwanted outcomes.

##### How to reproduce:
1. Start the server
2. Go to localhost:8080
3. Sign in (username: "ted", password: "ted")
4. When on form page enter "ted" as a name and "<script>alert('This is XSS!')</script>" as an address
5. Press submit and receive confirmation message about successful sign in for the event
6. Navigate to localhost:8080
7. When logged in enter "ted" as a name and "address" as an address
8. Now the browser runs the submitted script that was stored earlier (see section 4)

###### How to Fix:
All input parameter values should be validated before saving. This can be done by using javascript to sanitize the input before sending its values. The goal is that the input does not accept "<" or ">" characters. Thus, for example, the script tag cannot be entered.


### FLAW 3

#### 2. Broken Authentication

##### Description:
Authentication is broken for many reasons. First of all, there is not any verification before resetting password so possible attackers may have a change to access to the account. Another flaw is the visibility of default accounts directly from the source code.

##### How to reproduce:
1. Start the server
2. Go to localhost:8080
3. Sign in (username: "ted", password: "ted")
4. When on form page click the "reset password"-link
5. Type in a new password and submit it

##### How to fix:
As you can see there is not any verification before password reset. This could be done by doing the original password check where the application asks the current password from
the user. Also some minimum requirements for the password should be necessary, like length, upper and lower case characters etc.


### FLAW 4

##### 6. Security Misconfiguration

###### Description:
Application does not have a proper security configuration. Username and password are not complex enough and therefore are easily hacked. Also the Csrf protection is disabled which makes attacks possible on the application. The last mention is the old Spring Framework version which becomes a security threat upon identification.


###### How to reproduce:
Case 1:
1. Take a look at the pom.xml. In rows 14-16 can be seen that the Spring Boot version is "1.4.2.RELEASE" which is the old version of Spring Framework.
Case 2:
1. http.header are disabled, which results in a large number of errors. Csrf check is disabled so application is vulnerable for Csrf attacks.

###### How To fix:
Case 1 can be fixed by updating the Spring Framework to its latest version. After that the server needs to be restarted.
Case 2 can be fixed by removing "http.headers().disable();" (row 24) and "http.csrf().disable();" (row 23) from \src\main\java\sec\project\config\SecurityConfiguration.java


### FLAW 5

#### 10. Insufficient Logging & Monitoring

##### Description:
When site has been hacked or other unwanted activity is in progress an appropriate logging system can detect this kind of activity. However, this application lacks all monitoring and therefore is vulnerable because tracking the attacks is difficult.

##### How to reproduce:
1. Start the server
2. Go to localhost:8080
3. There isn't any logging happening that would monitor the application.

##### How to fix:
For example "logging.file.path" property can be added to application's properties and it allows application to print information to the console. This is a service that Spring Boot offers.


### FLAW 6

#### 9. Using Components with Known Vulnerabilities

##### Description:
OWASP recommended that using vulnerable frameworks is not the right way to do. This current application is using 1.4.2.RELEASE of Spring Framework which is significantly outdated and therefore have many vulnerabilities.

##### How to reproduce:
1. Take a look at the pom.xml file. In rows 14-16 can be seen that the program is using the version 1.4.2.RELEASE.

##### How to fix:
Upgrading to a newer framework version will fix this vulnerability. Version can be changed by adding it to the pom.xml file.
