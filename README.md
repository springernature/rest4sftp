# rest4sftp

Bridge from RESTful api to SFTP and FTP.


Beautifully written in Kotlin by Asad, Tom and Uberto

# TL;DR - How it works

rest4sftp translate RESTful HTTP calls into FTP or SFTP calls.

You can decide the protocol and other options with cmd line args or env variables.

build and run it:
```$bash
git clone https://github.com/uberto/rest4sftp.git
./gradlew clean build
cd build/distributions
unzip rest4sftp-<version>.zip
rest4sftp-<version>/bin/rest4sftp --sftp --port 8080
```

use it:
```$bash
// get listing of Upload directory on ftp.myserver.com
curl -i -X GET http://localhost:8080/folder/Upload -H "FTP-Host:ftp.myserver.com" -H "FTP-Port:22" -H "FTP-User:johnsmith" -H "FTP-Password:****"

// upload myfile.ext to Upload directory on ftp.myserver.com
curl -i -X PUT --data-binary @myfile.ext http://localhost:8080/file/Upload/myfile.ext -H "FTP-Host:ftp.myserver.com" -H "FTP-Port:22" -H "FTP-User:johnsmith" -H "FTP-Password:****"

// retrieve myfile.ext from Upload directory on ftp.myserver.com
curl -i -X GET http://localhost:8080/file/Upload/myfile.ext -H "FTP-Host:ftp.myserver.com" -H "FTP-Port:22" -H "FTP-User:johnsmith" -H "FTP-Password:****" -o downloaded.zip
```

# Installation Instructions

the easiest way is to use the docker image which is configured in `Dockerfile` to start a server on local port 8080. You
can start the image on 127.0.0.1:9990 (or another port of your choice) by one of the following methods:

## Docker run from source:
```$bash
docker build -t uberto/rest4sftp .
docker run -p 127.0.0.1:9990:8080/tcp uberto/rest4sftp --sftp
```

## Docker run from latest docker hub image:
```$bash
docker pull uberto/rest4sftp
docker run -p 127.0.0.1:9990:8080/tcp uberto/rest4sftp --sftp
```

## Docker-compose run instruction:
1. create a docker-compose.yml as follows:
```$yml
version: 3

services:
  rest4sftp:
    image: "uberto/rest4sftp"
    port: 9990:8080
    command: --sftp
```
2. run the following to start the docker image:
```$bash
cd <to directory where docker-compose.yml was created>
docker compose up
```


# API Reference

## List of folder API operations

### GET /folder/foldername

Returns an `application/json; charset=utf8` representation of the remote folder's contents.

Returns 401 if invalid ftp server credentials were present in the request header.

Returns 404 if the folder does not exist.

##List of file API operations

### GET /file/path/to/filename

Returns an `application/octet-stream` of the file's contents.

Returns 401 if invalid ftp server credentials were present in the request header.

Returns 404 if the file path does not exist.

### PUT /file/path/to/filename

Returns 200 if the file was successfully uploaded. The file contents is specified as raw data in the request body. If
the file already exists in the remote server then it will be overwritten.

Returns 400 if there was a problem with the file contents or file path.

Returns 401 if invalid ftp server credentials were present in the request header.

### DELETE /file/path/to/filename

Returns 200 if the file was successfully deleted.

Returns 400 if there was a problem with the file contents or file path.

Returns 401 if invalid ftp server credentials were present in the request header.

## Request Headers

The following are required HTTP request headers:
* FTP-Host: the remote (s)ftp server's hostname or IP
* FTP-Port: the remote (s)ftp server's port (default is 21 for ftp, 22 for sftp)

And either:

* FTP-User: the name of the ftp account on the remote server
* FTP-Password: the password for the ftp account on the remote server

Or:

* Authorization: a basic auth header (RFC7617) containing the ftp account name and password

# Code design

The main fun is in the class `com.ubertob.rest4sftp.main.MainKt`

The main classes are: **TODO**

