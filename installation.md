#Install MongoDB Community Edition On Windows


## Get MongoDB Community Edition
### Download [MongoDB](https://www.mongodb.com/download-center#community)
To get the correct version of MongoDB, you need to check your Windows version.
check your Windows version:

    C:\>wmic os get osarchitecture

> Note: 32-bit versions of MongoDB only support databases smaller than 2GB and suitble only for testing and evaluation purpose.

The MongoDB version used here is MongoDB Community Edition 3.4.1 - Windows Server 2008 R2 64-bit and later, with SSL support x64


## Install MongoDB Community Edition

### Install Mongo

After download the .msi file, locate the MongoDB .msi file. Double-click the .msi file. Follow the instructions to go through the installation process.

> Note: To specify an installation directory, please choose "Custom" installation option.

MongoDB is self-contained and does not have any other system dependencies. You can run MongoDB from any folder you choose. It is better to save it to other driver instead of C driver if you have no right to modify the "c:\Data" file.

Here, the MongoDB is installed to "H:\MongoDB".

## Run MongoDB Community Edition

### Environment setup

MongoDB requires a data directory to store all data. The default diretory path is the absolute path \data\db on the drive from which you start MongoDB. Create the folder by running the following command in **Git Bash**.

    $ mkdir h:/data/db

If you want to delete the directory you just created, try the following command

    $ rm -rf h:/data/db
    $ rm file_name //to delete files

Then you can specify an alternate path for data files using the *--dbpath* option to **mongod.exe**, for example:

    $"path/to/MongoDB path/bin/mongod.exe" --dbpath h:/path/to/data/db

If your path includes spaces, remember to enclose the entire path in double quotes like the first one.

### Start MongoDB

To start MongoDB, run **mongod.exe**. For example, from the **Git Bash**:

    $ ./mongod.exe

Umm, assuming you are in the right directory, if not, add the path to **mongo.exe**.

>Note: run **mongod.exe** instead of **mongo.exe**

Waiting for a while until the **waiting for connections on port 27017** in the console output which indicates the success of **mongod.exe** process.

    2017-01-23T11:37:04.485+0100 I NETWORK  [thread1] waiting for connections on port 27017


### Connect to MongoDB

Open a new **Git Bash** and run the **mongo.exe**

    $ ./h/MongoDB/Server/3.4/bin/mongo.exe
    MongoDB shell version v3.4.1
    connecting to: mongodb://127.0.0.1:27017
    MongoDB server version: 3.4.1

To disconnect MongoDB, run:

    exit

Now you are connecting with mongoDB. Congra!

### Begin using MongoDB

Later, to stop MongoDB, press Ctrl+c in the bash where the mongod instance is running.


## Configure a Windows Service for MongoDB Community Edition

>Important: please run your **Git Bash** as Administrator by right click your Bash and run as administrator.

In case you couldn't change directory to h driver directory. Try it

    $cd /h h:\MongoDB

### Create directories

Create directories for your database and log files:

    $mkdir h:/data/test
    $mkdir h:/data/test/db
    $mkdir h:/data/test/log

### Create a configuration file 

Create a configuration file which set systemLog.path and optional for storage.dbPath.

    $ vi > mongod.cfg
    
    systemLog:
        destination: file
        path: h:\data\test\log\mongod.log
    storage:
        dbPath: h:\data\test\db

Press ESC to exit editing and type :wq to save and exit the file.
>Note don't use tab between the systemLog.path and storage.dbPath keys and their values, use spaces. Otherwise, there will be parsing error.

### Install the MongoDB service

Install the MongoDB service by starting **mongod.exe** with the --install option and the -config option to specify the configuration file just be created previously.

    $Path/to/mongod.exe --config Path/to/mongod.cfg --install

### Start the MongoDB service

    net start MongoDB

Hey, stop now since I couldn't run **Git Bash** as administrator and change directory to network driver.