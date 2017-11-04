# Click server
This is the golang backend for the THS Armada [click application](https://github.com/armada-ths/click-frontend) 

# Dependencies
To run locally, you need to install golang.
## Mac
```
brew install golang
```

# Local installation
Golang has their own $GOPATH where you should store this project. 
This is usually ~/go/ after installing like above, where ~ refers to your home directory. 
Instead of cloning this repository, you write
```
go get github.com/armada-ths/click-backend
```
This should get the code into your $GOPATH which is probably ~/go/src/github.com/armada-ths/click-backend

Go to the source directory
```
cd ~/go/src/github.com/armada-ths/click-backend
```

Install all depenencies for the project and build the binary file
```
go install
go build -o click
```

## Run the server locally
The go install command should create a binary file called click. This file is the one we use to run the server.
First, we have to initialize the database that the server needs.

### Initialize database
```
./click init
```
Then create a user for the database
```
./click createuser
```
This is the user that the client (Your actual end-users) should use later. Make sure to have a good username and password.
For local testing it does not matter that much, you can always create new users later and the users you create will not be copied into live production later.

### Run the server
```
./click run
```


# Production installation
This will guide you how to setup the server on a Digital Ocean server.

1. Create a new droplet with Ubuntu (or use an existing one) following documentation from Digital Ocean.
2. Connect to the server using ssh. Again follow documentation from digital ocean if you do not know how to do this.
3. Complie the go application for linux systems
```
env  GOOS=linux go build -o build/click
```
4. Copy files to your server. Could be anywhere, this example copies to the home directory of the user deployment
```
scp -r ./build/* deployment@SERVER_IP:~/
```
5. Connect to your server
```
ssh deployment@SERVER_IP
```
6. Initialize the database
```
./click init
./click createuser
```
In this step you create the user that your end users will login to the app with, make sure to choose some good username and password

7. Run the installation script
```
sudo bash server_install.sh
```
This should enable the script as a service. Meaning it automatically restarts when server restart.
If not working, run each line inside the server_install script separate in the server terminal

8. Now your server is running and listening to port 3001 (default configuration). Go to SERVER_IP:3001 and you should get a small text saying 404 page not found.


## Setting up SSL
In the previous step, you had the server up and running without SSL. 
This section will guide you to set up nginx as a websocket proxy to force ssl connection oven the websocket as well. 

1. Install nginx
```
sudo apt-get update
sudo apt-get install nginx
```
2. Adjust your firewall
```
sudo ufw allow 'Nginx FULL'
sudo ufw allow 'OpenSSH'
sudo uft enable
```
Check what is now allowed with
```
sudo ufw status
```
3. Install certbot
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
```

```
sudo ln -s /etc/nginx/sites-available/{virtual host} /etc/nginx/sites-enabled/
sudo certbot --nginx -d SERVER_NAME
```
