# Getting Started with Terraform
Terraform is an infrastructure tool for defining and provisioning infrastructure as code (IaC). You can version, reuse, and share your stack with any platform or service that has an accessible API. 
This guide will run through how to initialize, plan, provision, and destroy a NGINX server inside a Docker container.

<br>

## Prerequisites

+ [Docker](https://www.docker.com/products/docker-desktop/)
+ [Terraform](https://www.terraform.io/downloads.html)

<br>

## Terraform Tutorial

Verify you are able to view terraform commands by running `terraform`. Now, you can start creating some infrastructure.

We recommend creating a new directory on your local machine and creating your Terraform configuration code inside it. 

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file and save the changes.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Initialize Terraform with the `init` command. This will install the AWS provider. 

```shell
$ terraform init
```
**Init Output**
<br>
```shell
Terraform has been successfully initialized!
```



Preview what actions Terraform will take.

```shell
$ terraform plan
```
**Plan Output**
<br>
```shell
Plan: 2 to add, 0 to change, 0 to destroy.
```

Address errors before continuing. Provision the resource with the `apply` command.

```shell
$ terraform apply
```
Type `yes` and hit ENTER.

**Apply Output**
<br>
```shell
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

The command will take a few minutes to run. Visit [localhost:80](localhost:80) to verify your NGINX server is running.
![terraform has successfully installed](https://content.hashicorp.com/api/assets?product=tutorials&version=main&asset=public%2Fimg%2Fterraform%2Fgetting-started%2Fterraform-docker-nginx.png)

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

Confirm you want to destroy all resources. Type `yes` and hit ENTER. 

**Destroy Output**
<br>
```shell
Destroy complete! Resources: 2 destroyed.
```

## Next Steps

You have installed Terraform and learned how to initialize, plan, provision, and destroy a NGINX web server.
<br>
Now you can now create infrastructure in the cloud.

+ [Amazon Web Services (AWS)](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build)
+ [Azure](https://developer.hashicorp.com/terraform/tutorials/azure-get-started/azure-build)
+ [Docker](https://developer.hashicorp.com/terraform/tutorials/docker-get-started/docker-build)
+ [Google Cloud Platform (GCP)](https://developer.hashicorp.com/terraform/tutorials/gcp-get-started/google-cloud-platform-build)
+ [Oracle Cloud Platform (OCI)](https://developer.hashicorp.com/terraform/tutorials/oci-get-started/oci-build)