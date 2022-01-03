terraform-provider-inwx
==========================

Terraform provider for INWX (InterNetworX)

## Description

With this custom terraform provider plugin you can manage your INWX domains.

## Usage

You could either install it manually on your workstation or use it from the [terraform registry](https://registry.terraform.io/providers/ofzhur/inwx/latest).

### manual installation

If you want to install it manually:

1. [Download](https://github.com/andrexus/terraform-provider-inwx/releases) the
   compiled plugin and make the file executable (`chmod +x terraform-provider-inwx`).  
   Or if you have Go installed:
    ```
    go get github.com/andrexus/terraform-provider-inwx
    go install github.com/andrexus/terraform-provider-inwx
    ```
2. Add plugin binary to your ~/.terraformrc file
    ```
    providers {
       inwx = "/path/to/your/bin/terraform-provider-inwx"
    }
    ```

### Provider Configuration

Since @ofzhur published this to the [terraform registry](https://registry.terraform.io/providers/ofzhur/inwx/latest), we are able to use it so.

```
terraform {
  required_providers {
    inwx ={
     source = "ofzhur/inwx"
    }
  }
}

provider "inwx" {
  username = "${var.inwx_username}"
  password = "${var.inwx_password}"
  sandbox  = "${var.inwx_sandbox}" // default is false
  tan      = "${var.inwx_tan}"     // if 2-Factor authentication is enabled for your INWX account
}
```

### DNS Records management
```
// example mx record
resource "inwx_record" "example" {
  domain   = "example.com"
  name     = ""
  type     = "mx"
  value    = "mx.example.com"
  ttl      = 3600
  priority = 10
}

// example A record
resource "inwx_record" "example" {
  domain   = "example.com"
  name     = "www" // subdomain e.g. www.example.com
  type     = "A"
  value    = "1.2.3.4"
  ttl      = 3600
  priority = "" 
}

```

#### Variables definition in  variables.tf
```
variable inwx_username {
 type = string
 description = "user provided inwx username"
}

variable inwx_password {
  type = string
  description = "user provided inwx password"
}

variable "inwx_tan" {
  type = string
  description = "user provided inwx 2fa tan"
}

variable "inwx_sandbox" {
  default = false
  type = string
  description = inwx sanbox"
}
```

#### Secrets definition in terraform.tfvars
```
inwx_username = "username"
inwx_password = "password"
```

If you don't specify the value for _inwx_tan_ variable (which you normally should not do
because TAN is only valid for 30 seconds) terraform will prompt you to input the TAN in command line.
Alternatively you can pass this variable as a command line parameter.
```
terraform plan -var 'inwx_tan=545817'
```
