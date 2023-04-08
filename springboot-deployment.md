## Running a Spring Boot Application on EC2

1. Install Java JRE 11:

`sudo apt-get install openjdk-11-jre`


2. Open a terminal on your EC2 instance and navigate to the `/etc/systemd/system` directory.

3. Create a new file called `your-app-name.service` using a text editor such as vim:

` sudo vi demo.service `

4. Add the following configuration to the file:
```
[Unit]
Description=Your Spring Boot Application
After=syslog.target

[Service]
User=ubuntu
ExecStart=/usr/bin/java -jar /home/ubuntu/app/app.jar
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target

```

5. Copy the jar file to the server:


`scp -i [path to pem file] [path to yelp.pgsql] username@[server-ip]:[directory to copy file to]`

6. Remember the jar file path: in my case


` /home/ubuntu/my-demo-app/server/spring-boot-uh-library-0.0.1-SNAPSHOT.jar`

7. To manually run the jar file, use the command in the directory that has the file


`java -jar filename.jar`
However we want to run it automatically using systemd. Stop it using `CRT+C`

8. Reload the systemd daemon to pick up the new service:
Reload the systemd daemon to pick up the new service:
 
`sudo systemctl daemon-reload`

9. Make it enabled

`sudo systemctl enable demo.service`

10. Check the status of the service:
 ```
 sudo systemctl status your-app-name.service
 ```

11. Retart
   `sudo systemctl restart demo.service`


Remember to configure security, including CORS.
