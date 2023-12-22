# To Install OWASP-ZAP (Zed Attack Proxy)

## Update and Upgrade:
Start by updating and upgrading package lists to ensure you have the latest package information.

    sudo apt update
    sudo apt upgrade -y


## Install Java:
OWASP ZAP requires Java to run. Install OpenJDK with the following command:

    sudo apt install default-jre -y


## Download and Install OWASP ZAP:
You can download and install OWASP ZAP using the following steps:
 - Download the latest OWASP ZAP package:

       wget https://github.com/zaproxy/zaproxy/releases/download/v2.13.0/ZAP_2.13.0_Linux.tar.gz

- Extract the downloaded package:

      tar -xzf ZAP_2.13.0_Linux.tar.gz

- Move the extracted directory to a suitable location (e.g., `/opt`):

      sudo mv ZAP_2.13.0 /opt/

     
## Run OWASP ZAP:
You can now launch OWASP ZAP by searching for it in your applications menu or by using the terminal:

    /opt/ZAP_2.13.0/zap.sh


### Run ZAP in Daemon Mode:

    /opt/ZAP_2.13.0/zap.sh -daemon -host 0.0.0.0 -port 8080

This starts ZAP in daemon mode, allowing you to interact with it through its API and web interface. You can access the ZAP web interface by navigating to 
http://<your-ec2-instance-ip>:8080 in your web browser.

### Run ZAP Inline:
If you're not planning to use the ZAP GUI or web interface, you can run ZAP inline without starting its GUI components. This is suitable for automated scanning or scripting purposes.

    /opt/ZAP_2.13.0/zap.sh -cmd
    
This command runs ZAP in command-line mode, allowing you to use its various command-line options for automated scanning and testing.

## For WEBUI

- Access ZAP Web UI:

To access the ZAP Web UI, you need to set up an SSH tunnel to forward the remote ZAP Web UI port to your local machine.
Keep your SSH session open and open a new terminal window on your local machine.

In the new terminal, create the SSH tunnel:

    ssh -i /path/to/your-key.pem -L 8080:localhost:8080 ubuntu@your-ec2-instance-ip -N

Replace 8080 with the desired local port. This command forwards the ZAP Web UI port (default is 8080) from the EC2 instance to your local machine.

- Access ZAP Web UI:
    Open a web browser on your local machine and navigate to:

        http://localhost:8080/
