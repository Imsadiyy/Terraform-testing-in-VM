✅ Step 1: Install Terraform

(Make sure you have it on your VM — I can re-share the commands if you haven’t installed yet).

✅ Step 2: Local File Example (Hello World)

This one just creates a local file to prove Terraform works.

main.tf:

```
provider "local" {}

resource "local_file" "hello" {
  filename = "${path.module}/hello.txt"
  content  = "Hello Terraform!"
}
```

Run:

```
terraform init
terraform apply -auto-approve
```

Check:

cat hello.txt

✅ Step 3: Manage Docker Containers with Terraform

You already used Docker 🚀. Terraform can manage Docker containers too.

Create a new folder:

```mkdir terraform-docker && cd terraform-docker```


Make main.tf:

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name = "nginx:latest"
}

resource "docker_container" "nginx" {
  name  = "nginx_server"
  image = docker_image.nginx.latest
  ports {
    internal = 80
    external = 8080
  }
}
```

Run:

```
terraform init
terraform apply -auto-approve
```

✅ This will pull the nginx image and run it in a container on port 8080.
Test in browser: http://<VM_IP>:8080

✅ Step 4: Destroy Resources

To stop & clean up:

```terraform destroy -auto-approve```
