# Iac (Infrastructure as Code)

## Install Terraform 
Ensure that your system is up to date and you have installed the gnupg, software-properties-common, and curl packages installed. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository.

    sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

## Install the HashiCorp GPG key.

    wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg

## Verify the key's fingerprint.

    gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint

## Add the official HashiCorp repository to your system.
The lsb_release -cs command finds the distribution release codename for your current system, such as buster, groovy, or sid.

    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list

## Download the package information from HashiCorp.

    sudo apt update

## Install Terraform from the new repository.

    sudo apt-get install terraform

## Verify the installation
Verify that the installation worked by opening a new terminal session and listing Terraform's available subcommands.

    terraform -version

## Enable tab completion
If you use either Bash or Zsh, you can enable tab completion for Terraform commands. To enable autocomplete, first ensure that a config file exists for your chosen shell.

Bash
    
    touch ~/.bashrc

## Then install the autocomplete package.

    terraform -install-autocomplete

Once the autocomplete support is installed, you will need to restart your shell.

============================================================================================

## Install Git:
Use the following command to install Git:

    sudo apt install git

## Verify Installation:
After the installation is complete, you can verify that Git is installed by checking its version:

    git --version












