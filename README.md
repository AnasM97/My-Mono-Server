# My-Mono-Server


### Creating an EC2 Instance and Installing Python inside the Server
- When to EC2 and created an instance
- Made my AMI(Amazon Machine Image) ubuntu and my instance type t3.micro
- I allowed SSH and HTTP Traffic from anywhere(0.0.0.0) aswell
- I went to my ssh folder on VS Code in the terminal and then ssh into my EC2 Instance
- I updated and upgraded the packages inside ubuntu
- I proceeded to make a new folder and change directory to that folder named myapp which is where my application is going to be stored
- Initially, I attempted to install python and then install flask but I could not do that and the reason was due to version clash as the Python Ubuntu uses is locked
- This is because core system features depend on a specific Python version so updating it would break those features
- Therefore the OS manager prevents the replacement of that Python Version
- The solution I used was creating a Virtual Environment with its own Python version that would not clash with the OS' Python

#### Flask
- After creating my virtual environment I was able to now install flask
- I then created a file named app.py where my application code is going to be stored
- I also needed to create a new security group with port 5000 that could be connected from anywhere
- A mistake I made with flask is that I had to run flask without stopping it as I kept on stopping it not knowing that by doing that my server connected to the browser simply would not connect

### Running Nginx
- When I initally wanted to start nginx after installing it, I had an error where [Warning: The unit file, source configuration file or drop-ins of nginx.service changed on disk.].
-  systemd(Linux Service Manager) warned that its service configuration file had changed.
- Running systemctl daemon-reload refreshes systemd’s knowledge of service definitions(A configuration file that tells Linux how to run a background program.) so it can properly start nginx.
- Started nginx and enbabled it to make it active such that when I go to http://Public-Server-IP, I recieved a welcome to nginx page.
- Editing nginx config files from showing a staticl HTML file in a folder to the page from my flask program. 
- What occurs from the change is that whenever someone visits the server, its going to forward a request to my flask app running locally on port 5000
- Flask runs inside my server on port 5000 and is not meant to be exposed direcly to the internet whereas Nginx is more secure and faster for production use and runs on port 80(Standard web port for HTTP web traffic) and therefore optimised for handling internet traffic. 
- User -> Nginx -> Flask
- This is known as a reverse proxy.
  - A server that receives requests and forwards them to another internal server.
- A way to test the configuration before restarting is sudo nginx -t and the response it should give is config syntax is ok + test is successful.
-  Now that I am running http://Public-Server-IP and it leads straight into the flask program.

  ## Adding a Database
- A database is needed to store all data from your app such as users,emails,passwords etc permanently.
- Adding SQLite to app.py allows the Flask application to store and retrieve persistent data using SQL.
- The app connects to a database file, creates a table if necessary, updates a visit counter on each request, saves the changes, and returns visit counter to the user.
- (I forgot to restart nginx but nginx had no conflict of interest with me building a database)



## Commands Used
- cd
- cd .ssh
- ssh -i "my-key-pair" ubuntu@mono-ip
- sudo apt update
- sudo apt upgrade -y
- mkdir myapp
- cd myapp
- sudo apt install python3 python3-venv
- python3 -m venv venv
- source/venv/bin/activate
- pip3 install flask
- nano app.py
  - from flask import Flask
  - app = Flask(__name__)
  - @app.route("/")
  - def home():
    return "Hello from my MONOLITHIC app running on EC2!"
  - if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
- python3 app.py
- sudo apt install nginx -y
- systemctl daemon-reload
- sudo systemctl start nginx
- sudo systemctl enable nginx
- systemctl status nginx
- sudo nano /etc/nginx/nginx.conf
  - location / {
    proxy_pass http://127.0.0.1:5000;#
  }
- sudo apt install sqlite3 -y
  - from flask import Flask
  - import sqlite3
  - app = Flask(__name__)
  
  - @app.route("/")
  - def home():
        - conn = sqlite3.connect("visits.db")
        - cur = conn.cursor()
        - cur.execute("CREATE TABLE IF NOT EXISTS counter (count INTEGER)")
        - cur.execute("SELECT count FROM counter")
        - row = cur.fetchone()
  
        if row is None:
            cur.execute("INSERT INTO counter VALUES (1)")
            count = 1
        else:
            count = row[0] + 1
            cur.execute("UPDATE counter SET count = ?", (count,))
  
        conn.commit()
        conn.close()
  
        return f"Hello! This page has been visited {count} times."
  
    if __name__ == "__main__":
        app.run(host="0.0.0.0", port=5000)

- 











### Keywords I Learnt
- Secure Shell(SSH) : A way to securely connect to another computer over a network and control it using the command line, Port 22.
- HyperText Transfer Protocol(HTTP) : It’s the system that allows your web browser and web servers to communicate and is not secured with encryption, Port 80.
- Virtual Environments : An isolated, self-contained directory that contains its own Python interpreter and installed packages, allowing a project to manage its dependencies independently from other projects and the system-wide Python installation.
- Flask : A lightweight web framework for Python used to build web applications and APIs, Port 5000.
- Reverse Proxy : A server that sits in front of your backend servers and handles incoming requests before they reach your application.
- 
