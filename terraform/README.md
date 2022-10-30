
## Formating code

```sh
terraform fmt
```

## Console
```sh
terraform init
terraform console
```

## Destroy target
```sh
terraform destroy -target resource.name
```

## Replace
```hcl
terraform apply -replace resource.name
# older versions
terraform taint resource.name
terraform apply
```
## Data sources
```hcl
data "aws_ami" "latest_amazon_linux2" {
    owners = ["amazon"]
    most_recent = true # optional => if more than one resource is return it will return the most recent
    

    filter {
        name = "name"
        values = ["amzn2-ami-kernel-*-x86_64-gp2"]
    }
    filter {
        name = "architecture"
        values = ["x86_64"]
    }
}

data.aws_ami.latest_amazon_linux2.id
```
## Terraform output
```sh
terraform output -json
terraform output ami_id
```



## Save error
```sh
#TF_LOG to TRACE DEBUG INFO WARN ERROR
export TF_LOG=DEBUG

#saving ourput to a file
export TF_LOG_PATH=terraform.log
```


## Complex types

```hcl
# collection: list map set

variable "a" {
    type = list(string)
    default = ["1", "2"]
}

variable "b" {
    type = map(string)
    default = {
        "key" = "value",
        "key2" = "value2"
    }
}

# usage
var.b["key"]

# structural: tuple, object



variable "a" {
    type = tuple([string,number,bool])
    default = ["1", 2, true]
}


# structur

variable "a" {
    type = object({
        from_port = number
        to_port = number

    })
    default = {
        from_port = 0,
        to_port = 65365
    }
}
```

## Count 

```hcl
#count is used to manage similar resources
resource "aws_iam" "test" {
    name = "user-${count.index}"
    count = 5
}
## it will create 5 iam service accounts
#---

variable "users" {
    type = list(string)
    default = ["demo-user", "admin1", "john"]

}
resource "aws_iam" "test" {
    name = "${element(var.users, count.index)}"
    count = "${length(var.users)}"
}

```

## For each 
```hcl

variable "users" {
    type = list(string)
    default = ["demo-user", "admin1", "john"]

}
resource "aws_iam" "test" {
    for_each = toset(var.users)
    name = each.key
}
```

## Dynamic Block

```hcl
variable "ïngress_ports" {
    description = "List Of Ingress Ports"
    type = list(number)
    default = [22,80,110,143,443,993, 8080]
}


resource "a" "b"{
    dynamic "ingress" {
        for_each = var.ingress_ports
        content {
            from_port = ingress.value
            to_port = ingress.value
            protocol = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
    }
}
# both are same config
resource "a" "b"{
    dynamic "ingress" {
        for_each = var.ingress_ports
        iterator = iport
        content {
            from_port = iport.value
            to_port = iport.value
            protocol = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
    }
}
```


## Conditional expressions

```hcl
variable "itest" {
    type = bool
    default = true
}

resource "aws_instance" "test-server" {
    ami = "ami-1213123232"
    instance_type = "t2.micro"
    count = var.istest == true ? 1:0
}

resource "aws_instance" "prod-server" {
    ami = "ami-1213123232"
    instance_type = "t2.micro"
    count = var.istest == false S? 1:0
}
```

## locals

```hcl

# they are just local, cannot be used with shared modules, because they remain local
locals {
    key = value
}

resource "a" "b"{
    local.value
}
```

## built-in functions

```txt
https://www.terraform.io/language/functions 

example

max(1,2)
2
```

## Using Splat(*) Expressions

```hcl
resource "aws_ubstabce" "server"{
    ...
}

output "private_addresses"{

    value = aws.server[*].private_ip
}
```

# save terraform state 
```hcl
#backend.tf

terraform {
    backend "s3"{ 
        bucket = "bucket"
        key = "path/to/my/key"
        region = "region"

    }
}

```

```sh
# migrate the Terraform state from local to remote and you add the correct backend block
terraform init --reconfigure
```