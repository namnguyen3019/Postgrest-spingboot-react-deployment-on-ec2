Deploy react app

1. install nginx

`sudo apt-get install nginx`

2. On your local machine, create build folder for you react app

`npm run build`

3. copy it to EC2

 `sudo scp -i key-value-pair-here.pem -r [react build folder] ubuntu@54.175.185.219:[directory on ec2 you want to put]`

on my machine it looks like this:

`sudo scp -i /Users/namnguyen/Downloads/bookstore-https.pem -r /Users/namnguyen/Projects/library-app/react-library-fontend/build/ ubuntu@54.175.185.219:/home/ubuntu/apps/bookstore-app/client`

4. Next, navigate to the /etc/nginx/sites-available/ directory and create a new configuration file for your React app:

`cd /etc/nginx/sites-available/`
`sudo vi myapp.conf`

5. config the server 
```
server {
    listen 80; // ip v4
    listen [::]:80; //ip v6

    server_name example.com www.example.com; # Replace with your domain or IP address

    root /var/www/myapp/build; # Replace with the path to your React app's build folder

    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri /index.html;
    }

    error_page 404 /index.html;

    location /api {
            proxy_pass http://localhost:8080;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
       }
}
```
6. 
Create a symbolic link from your configuration file in the sites-available directory to the sites-enabled directory:

`sudo ln -s /etc/nginx/sites-available/demo /etc/nginx/sites-enabled/`


7. Test the configuration file for syntax errors by running the following command:
`sudo nginx -t`

8.
If there are no syntax errors, restart Nginx to apply the changes:
`sudo systemctl restart nginx`

9. enable the Nginx service to start automatically on boot:

`sudo systemctl enable nginx`

