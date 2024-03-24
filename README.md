
## Mastering Infrastructure Automation with Terraform, Docker, and AWS 

Are you new to the world of infrastructure automation? Feeling overwhelmed by the complexity of setting up and managing your infrastructure? Fear not! Terraform is here to simplify your journey. In this beginner's guide, we'll walk you through the process of installing Terraform on a Linux Ubuntu system and provide hands-on examples to kickstart your infrastructure automation journey.

## Installing Terraform on Linux Ubuntu
RUN Below Command on Ubuntu

```bash
  wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

```
Verify Installation

```bash

    terraform version

```

## Hands-on Examples with Terraform

Now that Terraform is installed, let's dive into some practical examples to showcase its capabilities.


```bash
  mkdir terraform-practice
  cd terraform-practice
```
Create and Automate a File with Terraform
Now, let's create a file named main.tf and use Terraform to automate its creation:
```bash
  vim main.tf
```
Inside main.tf, add the following Terraform configuration to create a file named tws.txt with the specified content:

```bash
resource "local_file" "my_file" {
  filename = "tws.txt"
  content  = "This is a file created automatically by Terraform"
}
```
Save and exit the file by pressing Esc, then typing :wq, and pressing Enter.

To automate the creation of this file, follow these steps:
```bash
terraform init
terraform plan
terraform apply
```
After applying the configuration, you can verify that the file has been created by listing the contents of the directory:
```bash
ls
cat tws.txt
```##Automating Docker Environment Setup with Terraform
Terraform simplifies the process of setting up Docker environments through automation. Follow these steps to seamlessly integrate Docker into your infrastructure using Terraform:

Include Docker Provider in Terraform Configuration: Open your Terraform configuration file (e.g., terraform.tf) using a text editor. Paste the copied provider configuration code into your Terraform file under the terraform block.
```bash
 terraform {
   required_providers {
     docker = {
       source  = "Kreuzwerker/docker"
       version = "3.0.2"
     }
   }
 }
 provider "docker"{
     # Add your Docker provider configuration options here
     }
```

## Initialize Terraform: Initialize Terraform to download the Docker provider plugin:
```bash
 terraform init
```
Install Docker: Ensure Docker is installed on your system. If not, install Docker using the package manager:

```bash
 sudo apt-get update
 sudo apt-get install docker.io
 sudo chown $USER /var/run/docker.sock
```
## Define Docker Resources: Create a new Terraform configuration file (e.g., docker_project.tf) to define Docker resources. In this example, we'll create a Docker image and container:
```bash
vim docker_project.tf
```

Inside docker_project.tf, define Docker image and container resources:

```bash
resource "docker_image" "my-image" {
  name         = "trainwithshubham/node-app-batch-6:latest"
  keep_locally = false
}

resource "docker_container" "my_container" {
  image = "trainwithshubham/node-app-batch-6:latest"
  keep_locally = false
}

resource "docker_container" "my_container" {
  image = docker_image.my_image.name
  name  = "node-app"
  ports {
    internal = 8000
    external = 8000
  }
  depends_on = [docker_image.my_image]
}
```

## Apply Terraform Configuration: Apply the Terraform configuration to create Docker resources:

```bash
terraform plan
terraform apply
```
Verify Docker Environment: After applying the configuration, check the status of Docker containers:
```bash
docker ps
```

## Access Application: Access the application running in the Docker container via its assigned IP address and port:
ip:8000

By following these steps, you've automated the setup of a Docker environment using Terraform, making it easier to manage and deploy containers in your infrastructure.

Destroy Docker Environment: Once you're done with the Docker environment, you can destroy it to release resources:
```bash
terraform destroy
```

Confirm the destruction of resources when prompted. This command will remove all resources provisioned by Terraform, including Docker containers and images.
