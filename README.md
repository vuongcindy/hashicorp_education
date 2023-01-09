# Getting Started with Terraform

Terraform is an infrastructure tool for defining and provisioning infrastructure as code (IaC). You can version, reuse, and share your stack with any platform or service that has an accessible API. 
This guide will run through how to initialize, plan, provision, and destroy a NGINX server inside a Docker container.

## Prerequisites

+ [Docker](https://www.docker.com/products/docker-desktop/) v20.10.20
+ [Terraform](https://www.terraform.io/downloads.html) v1.3.7 

## Terraform Tutorial

Verify you are able to view Terraform commands by running `terraform`. Now, you can start creating some infrastructure.

### Create configuration

Create a new directory for your Terraform configuration. 

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file. Save the changes.

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

### Connect Terraform and Docker

Initialize Terraform with the `init` command. This will download a plugin allowing Terraform and Docker to interact.

```shell
$ terraform init
Initializing the backend...
Initializing provider plugins...

Terraform has created a lock file .terraform.lock.hcl to record the provider selections it made above.

Terraform has been successfully initialized!

You may now begin working with Terraform.
```

### View Execution Plan

Preview what actions Terraform will take with the `plan` command.

```shell
$ terraform plan
Terraform used the selected providers to generate the following execution plan. 

Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created

Plan: 2 to add, 0 to change, 0 to destroy.
```
Address errors before continuing. 

### Create Resources

Provision the resource with the `apply` command.

```shell
$ terraform apply
Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value:
```

Apply the configuration to provision the resources. Enter `yes` to confirm the action.

```shell
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

The command will take a few minutes to run. Visit [localhost:80](localhost:80) to verify your NGINX server is running.

![terraform has successfully installed](https://content.hashicorp.com/api/assets?product=tutorials&version=main&asset=public%2Fimg%2Fterraform%2Fgetting-started%2Fterraform-docker-nginx.png)

### Destroy Resources

Destroy the infrastructure with the `destroy` command.

```shell
$ terraform destroy
Terraform used the selected providers to generate the following execution plan.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value:
```

Destroy all resources. Enter `yes` to confirm the action.

```shell
Destroy complete! Resources: 2 destroyed.
```

## Next Steps

You have installed Terraform and learned how to initialize, plan, provision, and destroy a NGINX web server.

Now you can now create infrastructure in the cloud.
+ [Amazon Web Services (AWS)](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build)
+ [Azure](https://developer.hashicorp.com/terraform/tutorials/azure-get-started/azure-build)
+ [Docker](https://developer.hashicorp.com/terraform/tutorials/docker-get-started/docker-build)
+ [Google Cloud Platform (GCP)](https://developer.hashicorp.com/terraform/tutorials/gcp-get-started/google-cloud-platform-build)
+ [Oracle Cloud Platform (OCI)](https://developer.hashicorp.com/terraform/tutorials/oci-get-started/oci-build)