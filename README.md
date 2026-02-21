# My-Mono-Server

- To get into the virtual environment python in my directory is source/venv/bin/activate

### Flask




## Running Nginx
- When I initally wanted to start nginx after installing it, I had an error where [Warning: The unit file, source configuration file or drop-ins of nginx.service changed on disk.].
-  systemd(Linux Service Manager) warned that its service configuration file had changed.
- Running systemctl daemon-reload refreshes systemd’s knowledge of service definitions(A configuration file that tells Linux how to run a background program.) so it can properly start nginx.
- Started nginz and enbabled it to make it active such that when I go to http://Public-Server-IP, I recieved a welcome to nginx page.
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
- sudo apt update
- sudo apt upgrade -y
- mkdir myapp
- cd myapp
- 













### Keywords I Learnt
- 
