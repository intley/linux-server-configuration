# Linux Server Configuration
Linux Server Configuration involves taking a baseline installation of a Linux distribution on a virtual machine and prepare it to host your web applications, to include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.
In this case, the web application hosted was the Item Catalog application.

This project is part of the [Full Stack Web Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004) at _Udacity_.

## Server Details
The website can be accessed using the following URL's:

IP address: `
54.202.74.198`

SSH Port:`
2200`

URL: `http://54.202.74.198/` OR
 `http://ec2-54-202-74-198.us-west-2.compute.amazonaws.com/`

The Linux Server instance has been configured to be ssh into the port 2200, in addition to the default port of 22 in order to prevent any risks of ssh reconfiguration.

## Login Details for ssh (grader):
To login as the grader using ssh,
run the following command in your local terminal:
```
ssh grader@54.202.74.198 -p 2200 -i ~/.ssh/<privatekey>
```

The privatekey is paired with the public key stored on the server. In order to gain access, you must possess the private key.

#### Note to the Reviewer:
The private key contents will be provided in the Notes section for you to gain access.

A paraphrase has also been created for the user grader and this shall also be provided in the Notes section.

## Configuration changes
### Creating a new user:
A new user grader can be created using the following command:
```
sudo adduser grader
```

### Providing the grader sudo access:
Provide the user grader sudo access using the command:
```
sudo adduser grader sudo
```

You may verify that the user has been provided sudo access by run a simple sudo command such as:
```
sudo cat /etc/passwd
```

If your command returns an output containing user information for the linux instance, you can be certain that the grader has now been provided sudo access.

### Updating all currently installed packages:
To get information about all packages that are required to be updated, run the following command:
```
sudo apt-get update
```

The above command retrieves the update information along with the package indexes.

To update the packages with the retrieved information, run the following command:
```
sudo apt-get upgrade
```

If at login `*** System restart required ***` is displayed, run the following command to reboot the machine:
`reboot`

### Setting up a SSH key-pair for user grader:
To create a SSH key pair, run the following command in your local Terminal.
```ssh-keygen
```

Enter the directory where you wish to save the key-pair. It is recommended for the purpose of the project to store the file as follows:
`/Users/<User:Name>/.ssh/<filename>`

Inside this directory, once you run the command, you will find two files:
`<filename>.pub` and `<filename>`

`<filename>` is the private key which we will use to gain access to the server.

`<filename>.pub` is the public key which we must store in the server.

### Storing the public key in the server:
Login as the grader, and run the following commands to create the directory where we must store the public key:

```
sudo mkdir /home/grader/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/grader/.ssh/
chmod 700 /home/grader/.ssh
chmod 644 /home/grader/.ssh/authorized_keys
```
The chmod commands create the file permissions for the `.ssh` directory and the `authorized_keys` file.

Edit the authorized_keys file and append the contents of `<filename>.pub` to this file.

If you wish to be able to ssh into this linux server instance using the user ubuntu, follow the above steps similarly changing the path.

 You should now be able to login as:
 `ssh grader@54.202.74.198 -p 22 -i ~/.ssh/<filename>`
