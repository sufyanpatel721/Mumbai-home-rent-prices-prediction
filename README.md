# Mumbai-home-prices
## Table of Content
  * [Demo](#demo)
  * [Overview](#overview)
  * [Deploy this app to cloud (AWS EC2)](#Deploy-this-app-to-cloud(AWS EC2))
  * [Installation](#installation)
  * [Run](#run)


## Demo
![](https://github.com/sufyanpatel721/Mumbai-home-prices/blob/master/ezgif.com-video-to-gif.gif)

## Overview
This data science project series walks through step by step process of how to build a real estate price prediction website. A model was created using sklearn and linear regression using Mumbai home rent prices dataset from kaggle.com. Second step a python flask server that uses the saved model to serve http requests. Third component is the website built in html, css and javascript that allows user to enter home square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price. During model building the project has covered all data science concepts such as data load and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, gridsearchcv for hyperparameter tunning, k fold cross validation etc. Technology and tools this project covers,

1. Python
2. Numpy and Pandas for data cleaning
3. Matplotlib for data visualization
4. Sklearn for model building
5. Jupyter notebook, visual studio code and pycharm as IDE
6. Python flask for http server
7. HTML/CSS/Javascript for UI

## Installation
The Code is written in Python 3.7. If you don't have Python installed you can find it [here](https://www.python.org/downloads/). If you are using a lower version of Python you can upgrade using the pip package, ensuring you have the latest version of pip. To install the required packages and libraries, run this command in the project directory after [cloning](https://www.howtogeek.com/451360/how-to-clone-a-github-repository/) the repository:
```bash
pip install -r requirements.txt
```

## Deploy this app to cloud (AWS EC2)
1. Create EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Now connect to your instance using a command like this,
```
	ssh -i "C:\Users\sufyan\.ssh\mhp.pem" ubuntu@ec2-3-135-249-69.us-east-2.compute.amazonaws.com
```
3. nginx setup

I. Install nginx on EC2 instance using these commands,
```
	sudo apt-get update
	sudo apt-get install nginx
```
II. Above will install nginx as well as run it. Check status of nginx using


```
	sudo service nginx status
```
III. Here are the commands to start/stop/restart nginx
```
	sudo service nginx start
	sudo service nginx stop
	sudo service nginx restart
```
4. Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.Now you need to copy all your code to       EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: 
```
   https://winscp.net/eng/download.php
```
5. Once you connect to EC2 instance from winscp (instruction in a youtube video), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder   is now:
```
   **/home/ubuntu/BangloreHomePrices**
```
6. After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,

I. Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
```
   server {
   	listen 80;
        server_name bhp;
        root /home/ubuntu/Mumbaihomeprices/client;
        index app.html;
        location /api/ {
        	rewrite ^/api(.*) $1 break;
                proxy_pass http://127.0.0.1:5000;
               }
    	}
```
II. Create symlink for this file in /etc/nginx/sites-enabled by running this command,
```
    sudo ln -v -s /etc/nginx/sites-available/mhp.conf
```
III. Remove symlink for default file in /etc/nginx/sites-enabled directory,
```
   sudo unlink default
```
IV. Restart nginx,
```
   sudo service nginx restart
```
7. Now install python packages and start flask server
```
  sudo apt-get install python3-pip
  sudo pip3 install -r /home/ubuntu/Mumbaihomeprices/server/requirements.txt
  python3 /home/ubuntu/Mumbaihomeprices/client/server.py
```
Running last command above will prompt that server is running on port 5000.

8. Now just load your cloud url in browser (for me it was http://ec2-3-133-88-210.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment



