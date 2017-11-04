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
env  GOOS=linux go build -o click
```

