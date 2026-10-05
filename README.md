# Task 3 – Infrastructure as Code (IaC) with Terraform

Provisioning and managing a local Docker container using **Terraform**.

## Table of Contents

- [Objective](#objective)
- [Tools Used](#tools-used)
- [Project Structure](#project-structure)
- [Terraform Configuration](#terraform-configuration)
- [Workflow](#workflow)
  - [1. Initialize](#1-initialize)
  - [2. Validate](#2-validate)
  - [3. Plan](#3-plan)
  - [4. Apply](#4-apply)
  - [5. Verify the Docker Container](#5-verify-the-docker-container)
  - [6. Verify the Nginx Application](#6-verify-the-nginx-application)
  - [7. Inspect Terraform State](#7-inspect-terraform-state)
  - [8. Destroy](#8-destroy)
- [Commands Reference](#commands-reference)
- [Execution Logs](#execution-logs)
- [Result](#result)
- [Interview Questions](#interview-questions)
- [Task Information](#task-information)

---

## Objective

Understand **Infrastructure as Code (IaC)** by using Terraform to define a Docker infrastructure, create a container, inspect the Terraform state, and finally destroy the infrastructure.

## Tools Used

- **Terraform**
- **Docker**
- **Git**
- **GitHub**
- **Nginx Docker Image**

## Project Structure

```text
terraform-docker-task/
│
├── main.tf
├── README.md
│
├── execution-logs/
│   └── terraform-execution.txt
│
└── screenshots/
    ├── Required-Tools-version.png
    ├── terraform-init.png
    ├── terraform-validate.png
    ├── terraform-plan.png
    ├── terraform-apply.png
    ├── docker-container.png
    ├── nginx-browser.png
    ├── terraform-state.png
    └── terraform-destroy.png
```

## Terraform Configuration

The `main.tf` file defines the Docker provider, the Docker image, and the Docker container.

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  name  = "terraform-nginx"
  image = docker_image.nginx.image_id

  ports {
    internal = 80
    external = 8080
  }
}
```

This creates an Nginx container named `terraform-nginx` and exposes it on port `8080`.

---

## Workflow

### 1. Initialize

Initialize the Terraform working directory:

```bash
terraform init
```

Terraform downloads and configures the required Docker provider.

![Terraform Init](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-init.png)

### 2. Validate

Check the configuration before creating any infrastructure:

```bash
terraform validate
```

![Terraform Validate](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-validate.png)

### 3. Plan

Generate an execution plan showing the Docker image and container Terraform will create:

```bash
terraform plan
```

![Terraform Plan](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-plan1.png)
![Terraform Plan](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-plan2.png)

### 4. Apply

Provision the infrastructure:

```bash
terraform apply
```

Confirm the deployment by entering `yes` when prompted. Terraform then creates the Docker resources.

![Terraform Apply](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-apply1.png)
![Terraform Apply](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-apply2.png)

### 5. Verify the Docker Container

Confirm the container is running:

```bash
docker ps
```

The `terraform-nginx` container is up and running.

![Docker Container](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/docker-container.png)

### 6. Verify the Nginx Application

Open the application in a browser:

```text
http://localhost:8080
```

The Nginx welcome page confirms the container is running and accessible.

![Nginx Browser](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/nginx-browser.png)

### 7. Inspect Terraform State

List the resources managed by Terraform:

```bash
terraform state list
```

Inspect the full state:

```bash
terraform show
```

![Terraform State](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-state.png)

### 8. Destroy

Remove the infrastructure:

```bash
terraform destroy
```

Confirm by entering `yes` when prompted. Terraform destroys all resources it created.

![Terraform Destroy](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-destroy1.png)
![Terraform Destroy](https://github.com/Workwithaditya01/terraform-docker-task3/blob/74ddf0cf001cb318512b81b7ef38da4cf063a235/screenshots/terraform-destroy2.png)

---

## Commands Reference

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform state list
terraform show
terraform destroy
```

Docker was used to verify the container:

```bash
docker ps
```

## Execution Logs

Command outputs recorded during the task are stored in:

```text
execution-logs/terraform-execution.txt
```

## Result

The task was completed successfully using Terraform and Docker:

- [x] Terraform was initialized
- [x] The configuration was validated
- [x] An execution plan was generated
- [x] A Docker Nginx container was provisioned with Terraform
- [x] The running container was verified
- [x] The Nginx application was accessed at `localhost:8080`
- [x] Terraform state was inspected
- [x] The infrastructure was destroyed

This demonstrates the basic Infrastructure as Code workflow using Terraform with Docker.

---

## Interview Questions

### 1. What is IaC?

Infrastructure as Code (IaC) is the practice of defining and managing infrastructure using configuration files instead of creating it manually.

### 2. How does Terraform work?

Terraform reads configuration files, creates an execution plan, and then uses providers to create or modify infrastructure according to the configuration.

### 3. What is a Terraform state file?

The state file tracks the infrastructure resources managed by Terraform and their current state.

### 4. What is the difference between `terraform plan` and `terraform apply`?

- `terraform plan` previews the changes Terraform intends to make.
- `terraform apply` actually performs those changes.

### 5. What are Terraform providers?

Providers are plugins that let Terraform interact with different platforms and services. In this task, the Docker provider allows Terraform to manage Docker resources.

### 6. What is resource dependency?

A resource dependency exists when one resource requires another to exist first. Terraform uses dependencies to determine the correct order for creating and managing resources. (Here, `docker_container.nginx` depends on `docker_image.nginx`.)

### 7. How do you handle secret variables?

Sensitive information should never be hard-coded in Terraform files. Use variables, environment variables, secret-management systems, and protected Terraform variables to manage secrets securely.

### 8. What are the benefits of Terraform?

- Infrastructure as Code
- Repeatable deployments
- Version-controlled infrastructure
- Automated provisioning
- Consistent infrastructure
- Easy infrastructure changes
- Change previews with `terraform plan`

---

## Task Information

| | |
|---|---|
| **Task** | Task 3 – Infrastructure as Code (IaC) with Terraform |
| **Objective** | Provision a local Docker container using Terraform |
| **Tools** | Terraform, Docker |
| **Deliverables** | `main.tf` and execution logs |
| **Repository** | GitHub |