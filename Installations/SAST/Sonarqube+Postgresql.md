Increase the vm.max_map_count kernal ,file discriptor and ulimit for current session at runtime.

    sudo sysctl -w vm.max_map_count=262144
    sudo sysctl -w fs.file-max=65536
    ulimit -n 65536
    ulimit -u 4096

To Increase the vm.max_map_count kernal ,file discriptor and ulimit permanently . 
Open the below config file and Insert the below value as shown below,

    sudo nano /etc/security/limits.conf
    
    sonarqube   -   nofile   65536
    sonarqube   -   nproc    4096

Before installing, Lets update and upgrade System Packages

    sudo apt-get update
    sudo apt-get upgrade

Install wget and unzip package

    sudo apt-get install wget unzip -y

Step #1:Install OpenJDK 17 on Ubuntu 20.04 LTS
Install OpenJDK and JRE 11 using following command,

    sudo apt-get install openjdk-17-jdk -y
    sudo apt-get install openjdk-17-jre -y

SET Default JDK

To set default JDK or switch to OpenJDK enter below command,

    sudo update-alternatives --config java

You will see below choices for the alternative java (providing /usr/bin/java).
Type Index of version number to confirm

Check JAVA Version:

    java -version

Step #2: Install and Setup PostgreSQL 10 Database For SonarQube
Add and download the PostgreSQL Repo
    
    sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
    wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

Install the PostgreSQL database Server by using following command,

    sudo apt-get -y install postgresql postgresql-contrib

Start PostgreSQL Database server

    sudo systemctl start postgresql

Enable it to start automatically at boot time.
    
    sudo systemctl enable postgresql

Change the password for the default PostgreSQL user.
    
    sudo passwd postgres

Switch to the postgres user.

    su - postgres

Create a new user by typing:

    createuser sonar

Switch to the PostgreSQL shell.

    psql

Set a password for the newly created user for SonarQube database.

    ALTER USER sonar WITH ENCRYPTED password 'sonar';
    Create a new database for PostgreSQL database by running:
    CREATE DATABASE sonarqube OWNER sonar;
    grant all privileges to sonar user on sonarqube Database.
    grant all privileges on DATABASE sonarqube to sonar;
    \q

Switch back to the sudo user by running the exit command.

    exit
Step #3:How to Install SonarQube on Ubuntu 20.04 LTS
Download sonaqube installer files archieve To download latest version of visit SonarQube download page.

    cd /tmp
    sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip

Unzip the archeve setup to /opt directory

    sudo unzip sonarqube-9.9.0.65466.zip -d /opt

Move extracted setup to /opt/sonarqube directory

    sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube

Step #4: Configure SonarQube
We can’t run Sonarqube as a root user , if you run using root user it stops automatically.
We have found solution on this to create saparate group and user to run sonarqube.

1. Create Group and User:
Create a group as sonar

        sudo groupadd sonar

Now add the user with directory access

     sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar 
     sudo chown sonar:sonar /opt/sonarqube -R

Open the SonarQube configuration file.

    sudo nano /opt/sonarqube/conf/sonar.properties

Find the following lines.

    #sonar.jdbc.username=
    #sonar.jdbc.password=
    Uncomment and Type the PostgreSQL Database username and password which we have created in above steps and add the postgres connection string.
    /opt/sonarqube/conf/sonar.properties

And add these lines in file

    sonar.jdbc.username=sonar
    sonar.jdbc.password=sonar
    sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube

Edit the sonar script file and set RUN_AS_USER

    sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
    /opt/sonarqube/bin/linux-x86-64/sonar.sh

    # If specified, the Wrapper will be run as the specified user.
    
    # IMPORTANT - Make sure that the user has the required privileges to write
    
    #  the PID file and wrapper.log files.  Failure to be able to write the log
    
    #  file will cause the Wrapper to exit without any way to write out an error
    
    #  message.
    
    # NOTE - This will set the user which is used to run the Wrapper as well as
    
    #  the JVM and is not useful in situations where a privileged resource or
    
    #  port needs to be allocated prior to the user being changed.
    
     RUN_AS_USER=sonar
    Type CTRL+X to save and close the file.

2. Start SonarQube:
Now to start SonarQube we need to do following: Switch to sonar user

        sudo su sonar

Move to the script directory

    cd /opt/sonarqube/bin/linux-x86-64/
Run the script to start SonarQube

    ./sonar.sh start

3. Check SonarQube Running Status:
To check if sonaqube is running enter below command,

       ./sonar.sh status

4. SonarQube Logs:
To check sonarqube logs, navigate to /opt/sonarqube/logs/sonar.log directory

        tail /opt/sonarqube/logs/sonar.log

Step #5: Configure Systemd service
First stop the SonarQube service as we started manually using above steps Navigate to the SonarQube installed path

    cd /opt/sonarqube/bin/linux-x86-64/
Run the script to start SonarQube

    ./sonar.sh stop

Create a systemd service file for SonarQube to run as System Startup.
    
    sudo nano /etc/systemd/system/sonar.service

Add the below lines,

    [Unit]
    Description=SonarQube service
    After=syslog.target network.target
    
    [Service]
    Type=forking
    
    ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
    ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
    
    User=sonar
    Group=sonar
    Restart=always
    
    LimitNOFILE=65536
    LimitNPROC=4096

    [Install]
    WantedBy=multi-user.target

Save and close the file. 
Now stop the sonarqube script earlier we started to run using as daemon. 

Start the Sonarqube daemon by running:
    
    sudo systemctl start sonar

Enable the SonarQube service to automatically  at boot time System Startup.

    sudo systemctl enable sonar

check if the sonarqube service is running,

    sudo systemctl status sonar

Successfully, We have covered How to Install SonarQube on Ubuntu 20.04 LTS .

Step #6: Access SonarQube
To access the SonarQube using browser type server IP followed by port 9000.

    http://server_IP:9000 OR http://localhost:9000
