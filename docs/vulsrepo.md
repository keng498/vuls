---
id: vulsrepo
title: VulsRepo
sidebar_label: VulsRepo
---

[VulsRepo](https://github.com/usiusi360/vulsrepo) is awesome OSS Web UI for Vuls.
With VulsRepo you can analyze the scan results like Excel's pivot table.

<img src="https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/demo.gif" width="100%">

## Online Demo

http://usiusi360.github.io/vulsrepo/

## Requirements

- [future-architect/Vuls](https://github.com/future-architect/vuls) >= v0.4.0
- Web Browser : Google Chrome or Firefox

## Installation

*A home folder of vuls is explained as /opt/vuls.*

### Step1. Create a json report of vuls

```

$ cd /opt/vuls
$ vuls scan 
$ vuls report -format-json 

```

Output to a JSON files (/opt/vuls/results/)

### Step2. Installation

From now on, executed by a user running the vuls scan.

- Git clone

```

$ cd $HOME
$ git clone https://github.com/usiusi360/vulsrepo.git

```

### Step3. Change the setting of vulsrepo-server

Set Path according to your own environment.

```

$ cd $HOME/vulsrepo/server
$ cp vulsrepo-config.toml.sample vulsrepo-config.toml
$ vi vulsrepo-config.toml
[Server]
rootPath = "/home/vuls-eser/vulsrepo"
resultsPath  = "/opt/vuls/results"
serverPort  = "5111"

```

- Do not use the path of the symbolic link for resultsPath

### Step4. Start vulsrepo-server

```

$ pwd
$HOME/vulsrepo/server

$ ./vulsrepo-server 
2017/08/28 11:04:00 main.go:90: INFO: RootPath Load:  /root/work/vulsrepo
2017/08/28 11:04:00 main.go:97: INFO: ResultsPath Load:  /opt/vuls/results
2017/08/28 11:04:00 main.go:66: Start: Listening port: 5111

```

- It is necessary to build by yourself except for Linux 64bit. Please look at the build section.

### Step5. Always activate vulsrepo-server

##### Case: SystemV (/etc/init.d)

- Copy startup file. Change the contents according to the environment.

```

$ sudo cp $HOME/vulsrepo/server/scripts/vulsrepo.init /etc/init.d/vulsrepo
$ sudo chmod 755 /etc/init.d/vulsrepo
$ sudo vi /etc/init.d/vulsrepo

```

- Set to start automatically

```

$ sudo chkconfig vulsrepo on

```

- Start vulsrepo-server

```

$ sudo /etc/init.d/vulsrepo start

```

##### Case: systemd (systemctl)

- Copy startup file. Change the contents according to the environment.

```

$ sudo cp $HOME/vulsrepo/server/scripts/vulsrepo.service /lib/systemd/system/vulsrepo.service
$ sudo vi /lib/systemd/system/vulsrepo.service

```

- Set to start automatically

```

$ sudo systemctl enable vulsrepo

```

- Check settings

```

$ sudo systemctl list-unit-files --type=service | grep vulsrepo
vulsrepo.service                           enabled

```

- Start vulsrepo-server

```

$ sudo systemctl start vulsrepo

```

## Usage ##

Access the browser

```

http://<server-address>:5111

```

## DigestAuth

1. To perform digest authentication, create an authentication file.

```

$ ./vulsrepo-server -h
Usage of ./vulsrepo-server:
  -c string
        AuthFile Path (default "/home/vuls-user/.htdigest")
  -m    make AuthFile
  -r string
        realm (default "vulsrepo_local")
  -u string
        login user (default "vuls")

ex)
$ ./vulsrepo-server -m
Password: ****
AuthFile Path   :  /home/vuls-user/.htdigest
realm           :  vulsrepo_local
login user      :  vuls
2017/08/28 19:11:59 main.go:96: Create Success

```

2. Edit vulsrepo-config.toml.

```

$ vi vulsrepo-config.toml
[Auth]
authFilePath = "/home/vuls-user/.htdigest"
realm = "vulsrepo_local"

```

3. Start vulsrepo-server

## Use SSL

1. Create a self-signed certificate

```

$ openssl genrsa -out key.pem 2048

$ openssl req -new -x509 -sha256 -key key.pem -out cert.pem -days 3650

```

2. Edit vulsrepo-config.toml.

```

$ vi vulsrepo-config.toml
[Server]

serverSSL = "yes"
serverCert = "cert.pem"
serverKey = "key.pem"

```

3. Start vulsrepo-server


## Build vulsrepo-server

- It is necessary to build by yourself except for Linux 64bit
- Install golang beforehand.

```

$ mkdir -p $GOPATH/src/github.com/usiusi360/
$ cd $GOPATH/src/github.com/usiusi360/
$ git clone https://github.com/usiusi360/vulsrepo.git
$ cd vulsrepo/server
$ go get -u github.com/golang/dep/...
$ dep ensure
$ go build -o vulsrepo-server

```

## Misc

### SETTING

<img src="https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image006.png" width="80%">

- Pivot Table

| Setting Name |  |
|:-|:-|
|Show / Hide the Item|Set items not to be displayed on pivot table. Since the data of Summary is very large, it turns OFF when the number of vulnerabilities is large and an error occurs.|
|Priority|Set the data to be displayed with priority in the pivot table.The default is NVD data.|

### Sharing settings

- How to share pivot settings with others

<img src="https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image007.png" width="80%">

1. Click the link display button on the upper right.
2. Click the copy button to clickboard.
3. Pass the copied URL to another person.
4. When you access the URL in the browser and select the file, the pivot is displayed with the same setting.

### FAQ

- Why does not Total of Vuls and VulsRepo result match ?

Vuls aggregates the number of CveIDs included in the host.
However, VulsRepo counts Packages related to CveID as one case.
If more than one package is associated with one CveID, Total will increase more than Vuls.

<img src="https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image008.png" width="80%">

## Gallery

![image](https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image001.png)
![image](https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image002.png)
![image](https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image003.png)
![image](https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image004.png)
![image](https://raw.githubusercontent.com/usiusi360/vulsrepo/master/gallery/image005.png)
