# Docker & Nginx
Steps to dockerise grafana dashboard and deploy it in nginx(Tested on Ubuntu)

#### Installing and running docker
1. Install docker ```sudo apt install docker.io```
    
2. Pull and run the grafana docker image

    ```sudo docker run -d -p 3000:3000 --name=grafana grafana/garafana-oss```

3. Check whether the above command worked by running ```sudo docker ps``` (lists all the containers running)
4. You can access the grafana dashboard by typing ```127.0.0.1:3000``` in your browser.

#### Installing and running nginx
1. Install nginx ```sudo apt install nginx```
    
2. Enable nginx by running ```sudo systemctl enable nginx```
3. Now lets open up port 80 by running ```sudo ufw allow 'Nginx HTTP'```
4. Start nginx by running ```sudo systemctl start nginx```
5. Now go to your webbrowser and type ```127.0.0.1``` to see if its working perfectly or you can also do ```sudo systemctl status nginx``` from the terminal.

#### Making the docker image accessible via nginx
1. Use the command ```sudo nano /etc/nginx/sites-available/grafana``` and paste the snippet below onto the file that opens up

    ```
    server{
    listen 80;
    server_name 127.0.0.1;

    location /grafana {
    proxy_pass http://127.0.0.1:3000/;
      }  
    }
    ```
    
2. Save and close the file and create a symlink of the above file by running

    ```sudo ln -s /etc/nginx/sites-available/grafana /etc/nginx/sites-enabled```
        
3. Check if everything is working by running ```sudo nginx -t```
4. If the above command doesn't show any errors, reload nginx by running ```sudo systemctl reload nginx```
5. Now go to ```127.0.0.1/grafana``` to access your grafana dashboard.

### References
The steps discussed above are in a brief manner, if you need more details on any of the steps discussed above, you can refer to [this](https://grafana.com/grafana/download?edition=oss&platform=docker) and [this](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04) for installing the grafana docker image and setting up nginx respectively.
