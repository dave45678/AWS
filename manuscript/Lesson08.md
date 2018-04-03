# Lesson 08 - Host a Spring Boot Application

## Prerequisites
* Set up the database in the previous lesson.


## The Walkthrough

### Change the database connection properties
1. Open the application.properties the file is located under the ‘resource‘ directory
2. Change the properties as follows:

```java
spring.datasource.url=jdbc:mysql://<dbname>.<username>.us-east-1.rds.amazonaws.com:3306/<dbname>
spring.datasource.username=dbuser
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect
spring.jpa.hibernate.ddl-auto=update
```

3. Using your IDE export your project to a JAR file

4. Go to the AWS console. Select the *EC2 Service*
5. Select *instances* and click on the *Launch Instance* button. Select the free tier eligible instance and launch it
6. During the launch you will be prompted to create a security key
   * Select the ‘Create a new key pair‘
   * download the key and save it as ```mykeyfile.pem```

5. If you are using Mac OS or Linux change permissions on the file using the following command from the same directory as the file:

```cmd
chmod 400 mykeyfile.pem
```

6. You can now connect to the EC2 instance via terminal (command line). Use the ssh command that shows in the ‘Connect To Your Instance‘

```cmd
 ssh -i "mykeyfile.pem" ec2-user@ec2-**********.us-east-1.compute.amazonaws.com
```

7. It's probably a good idea to run the update command to get all the newest versions of packages and their dependencies.

```cmd
sudo apt-get update
```
8. Make sure you have Java version 8


```cmd
java -version
```

9. If you need Java 8 then you can install it by running the command below:

```cmd
sudo apt install default-jre
```

10. Using SFTP or other secure FTP software, upload the JAR file onto the EC2 instance.  A good location would be under the home directory in a directory called *spring-app*


11. From AWS, open port 8080 for the outside world
12. Select the 'Security Group' of the instance. Add following rule:


15) Finally, following command can be used to run the REST service.

```cmd
nohup java -jar spring-boot-1.0-SNAPSHOT.jar &
```




## What you should see

We can check whether the application is running or not look at the nohup.out file. Use the following command to check the nohup.out.
less nohup.out
When you click on the EC2 instance, you will be able to find the ‘IPv4 Public IP‘ that IP can be used to send a REST request.

### Useful Linux Commands

```cmd
ps -ef | grep spring – this command is useful check whether the application is running or not

kill -9 PID - replace PID with the process id from the command above to stop that command

rm nohup.out - use for delete nohup.out file

less nohup.out – open the nohup.out file if you want to view it
```




## What's Going On

Nohup command is helpful in when a user wants to start long running application log out or close the window in which the process was initiated. Either of these actions normally prompts the kernel to hang up on the application, but a nohup wrapper will allow the process to continue.  Both ```nohup myprocess.out &``` or ```myprocess.out &``` set myprocess.out to run in the background. Normally, when running a command using ```&``` and exiting the shell afterwards, the shell will terminate the sub-command with the hangup signal (```kill -SIGHUP <pid>```). This can be prevented using nohup, as it catches the signal and ignores it so that it never reaches the actual application.


## Questions
