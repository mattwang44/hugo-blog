---
title: "Terraform Learning Note"
subtitle: ""
description: "The note taken during my preparation of Terraform certification provided by HashiCorp."
excerpt: "The note taken during my preparation of Terraform certification provided by HashiCorp."
date: 2021-11-01
author: "Matt Wang"
image: "http://www.techelium.in/wp-content/uploads/2015/10/blog-banner.jpg"
published: true
tags:
  - IaC
  - Terraform
  - DevOps
URL: "/2021/11/01/terraform-notes/"
categories: ["Tech"]
---


## Learning Materials

- Terraform Official Document
- [Terraform Best Practice](https://www.terraform-best-practices.com/)
- Practice exams
  - [Bryan Krausen@Udemy](https://www.udemy.com/course/terraform-associate-practice-exam/)
  - [ExamPro](https://www.exampro.co/terraform)
- [Terrafrom Certificate](https://www.hashicorp.com/certification/terraform-associate)
  - [Study guide](https://learn.hashicorp.com/tutorials/terraform/associate-study)
  - [And I got one for myself](https://www.credly.com/badges/adcca0d4-b53f-4829-92b5-8262c53049f5)

## IaC Concept

- [What is IaC?](https://www.stackpath.com/edge-academy/what-is-infrastructure-as-code)

{{< figure src="https://images.prismic.io/alpacked/ae95673b-c8dd-42c4-a6a6-5b07ccef1200_tild3731-3961-4530-a366-306639623066__untitled-1_recovered.png?auto=compress,format" caption="Comparison among IaC tools by Alpacked" link="https://alpacked.io/blog/infrastructure-as-code-for-devops/">}}

## Terraform Basics

### Provider

- [Doc](https://www.terraform.io/docs/language/providers/configuration.html)
- example

  ```py
  terraform {
    required_providers {
      aws = {
        source = "hashicorp/aws"
        version = "3.58.0"
      }
    }
  }

  provider "aws" {
    # Configuration options
  }
  ```

- Use `alias` to set alternate providers

  ```py
  # reference this as `aws.west`
  provider "aws" {
    alias  = "west"
    region = "us-west-2"
  }
  ```

### Modules

- Root Module: A collection oof `.tf` files in the same directory.

  - Calling a child module in module

    ```python
    module "servers" {
      source = "./app-cluster" # local path of a child module

      servers = 5
    }
    ```

- Only one `terraform` block across a module.
- Meta-arguments: `count`, `for_each`, `providers` (use default if not specified), `depends_on`

#### Sources

- [Doc](https://www.terraform.io/docs/language/modules/sources.html)
- Source types
  - Terraform Registry ([registry.terraform.io](https://registry.terraform.io/browse/providers))
    - all public
    - supports [version constraint](https://www.terraform.io/docs/language/expressions/version-constraints.html)
  - HTTP URL or local path
  - Git (using HTTPS or SSH)/GitHub/BitBucket
  - Cloud Storage (e.g. S3, GCS, but Azure is not supported)
  - Terraform Cloud ([support private registry](https://www.terraform.io/docs/cloud/registry/index.html))

#### Variables & Output

- [Doc](https://www.terraform.io/docs/language/values/index.html)

### Provisioner

- Build images first. Provisioner is the last resort.
- [Connection Settings](https://www.terraform.io/docs/language/resources/provisioners/connection.html)
  - Applying it in resource block affects all provisioner in that resouce.
- `null_resource`

  - [Doc](https://www.terraform.io/docs/language/resources/provisioners/null_resource.html)
  - Use it when do not want to associate with resource

    ```python
    resource "null_resource" "null_resource_simple" {
        provisioner "local-exec" {
            command = "echo Hello World"
        }
    }
    ```

  - Use `triggers` argument to re-run

    ```c
    resource "aws_instance" "cluster" {
      count = 3
      # ...
    }

    resource "null_resource" "cluster" {
      # Changes to any instance of the cluster requires re-provisioning
      triggers = {
        cluster_instance_ids = "${join(",", aws_instance.cluster.*.id)}"
      }

      connection {
        host = "${element(aws_instance.cluster.*.public_ip, 0)}"
      }

      provisioner "remote-exec" {
        inline = [
          "bootstrap-cluster.sh ${join(" ", aws_instance.cluster.*.private_ip)}",
        ]
      }
    }
    ```

- Genric Provisioners

  - [file](https://www.terraform.io/docs/language/resources/provisioners/file.html)
    - copy files or directories (`source`) or specified content (`content`) to a created resource (`destination`).
  - [local-exec](https://www.terraform.io/docs/language/resources/provisioners/local-exec.html)
    - Invoke local executable after a resource is created.
  - [remote-exec](https://www.terraform.io/docs/language/resources/provisioners/remote-exec.html)
    - Invoke script on a remote resource after it's created.
    - `inline` runs inline commands while `script` & `scripts` copy one or multiple scripts to remote and execute.

  ```c
  resource "aws_instance" "web" {
    # ...

    provisioner "file" {
      content     = "ami used: ${self.ami}"
      destination = "/tmp/file.log"
    }

    provisioner "local-exec" {
      command = "echo $FOO >> sample.txt"
      environment = {
        FOO = "bar"
      }
    }

    provisioner "file" {
      source      = "script.sh"
      destination = "/tmp/script.sh"
    }

    provisioner "remote-exec" {
      inline = [
        "chmod +x /tmp/script.sh",
        "/tmp/script.sh args",
      ]
    }
  }
  ```


### Functions

- [Doc](https://www.terraform.io/docs/language/functions/index.html)
- TF _does not_ support user-defined functions.
- Collection Function
  ```sh
  > concat(["a", ""], ["b", "c"])
  ["a", "", "b", "c"]
  > slice(["a", "b", "c", "d"], 1, 3)
  [ "b", "c", ]
  > coalesce("", "b") # return first element not null or not empty string
  "b"
  > compact(["a", "", "b", "c"])
  ["a", "b", "c"]
  ```
- String Function
  ```sh
  > format("Hello, %s!", "Ander")
  > split(",", "foo,bar,baz")  # and join()
  > replace("1 + 2", "+", "-")
  ```
- IP Network Function

  ```sh
  > cidrhost("10.12.127.0/20", 16) # prefix, hostnum
  10.12.112.16

  > cidrnetmask("172.16.0.0/12")
  255.240.0.0  # 11111111 11110000 00000000 0000000

  > cidrsubnet("10.1.2.0/24", 4, 15) # prefix, newbits, netnum
  10.1.2.240/28   # 2**(8-4) * 15 = 240, 24 + 4 = 28

  > cidrsubnets("10.1.0.0/16", 4, 4, 8, 4)
  [ "10.1.0.0/20", "10.1.16.0/20", "10.1.32.0/24", "10.1.48.0/20",]

  > [for cidr_block in cidrsubnets("10.0.0.0/8", 8, 8) : cidrsubnets(cidr_block, 4, 4)]
  [
      [ "10.0.0.0/20", "10.0.16.0/20", ],
      [ "10.1.0.0/20", "10.1.16.0/20", ],
  ]
  ```


## Workflows

- [Doc](https://www.terraform.io/guides/core-workflow.html)
- Different Situation
  - Individual: Simply write -> plan -> apply.
  - Work as a team: Get the latest code (e.g. via git) -> write -> plan -> commit -> apply
  - [Core Workflow (Terraform Cloud)](https://www.terraform.io/guides/core-workflow.html#the-core-workflow-enhanced-by-terraform-cloud)

### Core Workflow & CLI

- [CLI Configuration File (.terraformrc)](https://www.terraform.io/docs/cli/config/config-file.html)
- `terraform init` ([doc](https://www.terraform.io/docs/cli/commands/init.html))
  - Mainly does 3 things:
    - Create a `.terraform` directory
    - Download plugin dependencies
    - Create a dependency lock file (named `.terraform.lock.hcl`, [doc](https://www.terraform.io/docs/language/dependency-lock.html#lock-file-location))
    - (Note that tf state is created when `terraform apply`)
  - `-upgrade` option upgrades plugins.
  - `-backend-config=backend.hcl` option enables users to store sensitive backend configs in another file.
- [Format and Validate](https://www.terraform.io/docs/cli/code/index.html)
  - [terraform console](https://www.terraform.io/docs/cli/commands/console.html)
    ```sh
    > echo "1 + 5" | terraform console
    6
    ```
  - [terraform fmt](https://www.terraform.io/docs/cli/commands/fmt.html)
    - for [style convention](https://www.terraform.io/docs/language/syntax/style.html)
    - `-write=true` to overwrite the files
  - [terraform validate](https://www.terraform.io/docs/cli/commands/validate.html)
    - Check if the required attributes are presented and the correct types are used for values.
- `terraform plan`
  - `-out=FILENAME` for exporting saved plan
- `terraform apply`
  - `-refresh-only` option (equivalent to `terraform refresh`) only updates the state.
  - `-replace=` option (equivalent to `terraform taint`) marks a particular object as needed to be replaced.
- `terraform destroy`
  - [Doc](https://www.terraform.io/docs/cli/commands/destroy.html)
  - Equivalent to `terraform apply -destroy` (for plan mode, use `terraform plan -destroy`)

### Debugging

- [Doc](https://www.terraform.io/docs/internals/debugging.html), [Troubleshoot tutorial](https://learn.hashicorp.com/tutorials/terraform/troubleshooting-workflow)
- Set `TF_LOG` env var to `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, or `JSON` (`TRACE` with json format).
  - [StackOverflow: When to use different log levels?](https://stackoverflow.com/questions/2031163)
- Set `TF_LOG_PATH` to append logs to a file.
- Check `crash.log` if TF has ever crashed.

## Implement and Maintain State

### Backend

- 2 types, `Standard` & `Enhanced`

  - [Doc](https://www.terraform.io/docs/language/settings/backends/index.html)
  - `Standard`: only store state, and rely on the local backend for performing operations (e.g. S3).
  - `Enhanced`: both store state and perform operations.
    - 2 enhanced backend types: `local` & `remote`.

  ```python
  # standard backend using s3
  terraform {
    backend "s3" {
      bucket = "my-terraform-state-bucket"
      key = "statefile"
      region = "us-east-1"
    }
  }

  # remote backend
  terraform {
    backend "remote" {
      hostname = "app.terraform.io"
      organization = "company"

      workspaces {
        prefix = "my-app-"
      }
    }
  }

  # it uses local backend by default
  terraform {
  }

  # local backend
  terraform {
    backend "local" {
      path = "relative/path/to/terraform.tfstate"
    }
  }
  ```

- Use `prefix` for multiple workspace
  - [Doc](https://www.terraform.io/docs/language/settings/backends/remote.html#configuration-variables)
  - example
    ```c
    workspaces {
      prefix = "app-"
    }
    ```

### State

- [Doc](https://www.terraform.io/docs/language/state/index.html)
- [Purposes](https://www.terraform.io/docs/language/state/purpose.html)
  - Mapping to real resources.
  - Metadata (e.g. dependencies)
  - Performance: Cache state to reduce queries for resources
  - Syncing for team collaboration.
- Stored in a local file `terraform.tfstate`. TF [refresh](https://www.terraform.io/docs/cli/commands/refresh.html) the file before any command.
  - Storing in remote works better for teamwork ([remote state](https://www.terraform.io/docs/language/state/remote.html)).
- [terraform state](https://www.terraform.io/docs/cli/commands/state/index.html)

  ```
  > terraform state --help
  Usage: terraform state <subcommand> [options] [args]
  ...
  Subcommands:
    list    List resources in the state
    mv      Move an item in the state
    pull    Pull current state and output to stdout
    push    Update remote state from a local state file
    rm      Remove instances from the state
    show    Show a resource in the state
  ```

  - rename resource: `terraform state mv <resource>.<old_name> <resource>.<new_name>`

- One should always treat state file as containing sensitive data.
  - Using a remote backend or Terraform Cloud to manage state is recommended.
  - [Suppress sensitive date in CLI output](https://www.terraform.io/docs/language/values/variables.html#suppressing-values-in-cli-output)
- [Locking](https://www.terraform.io/docs/language/state/locking.html)
  - lock state for ops that write state
  - [terraform force-unlock](https://www.terraform.io/docs/cli/commands/force-unlock.html) to unlock
- Backup
  - path: `terrraform.tfstate.backup`
- Sensitive data exists in the state
  - S3: add `encrypt` option
  - TF Cloud: always encrypts state

### Importing Resources

- `terraform import`: bring existing resources under Terraform's management
  - [doc](https://www.terraform.io/docs/cli/import/index.html), [cli doc](https://www.terraform.io/docs/cli/commands/import.html), [tutorial](https://learn.hashicorp.com/tutorials/terraform/state-import)
  - Not every resource is importable, checkout resource doc before importing.
  - Procedures
    1. Write a resource (body can be blank).
    2. Run `terraform import` (Note that TF will not update the script after importing)
    3. Checkout tf output if there is any secondary resource (e.g. `aws_network_acl_rule` for `aws_network_acl`). Create resource for each of them.
  - Examples
    - Import into resource: `terraform import aws_instance.foo i-abcd1234`
    - Import into resource with count: `terraform import 'aws_instance.baz[0]' i-abcd1234`
    - Import into module: `terraform import module.foo.aws_instance.bar i-abcd1234`

## Read, Generate, and Modify Configuration

### Data Block

- `filter`
  - [Doc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/subnet_ids#argument-reference)
  - example
  ```c
  data "aws_ami" "web" {
    filter {
      name   = "state"
      values = ["available"]
    }
    filter {
      name   = "tag:Component"
      values = ["web"]
    }
  }
  ```

- Inject secrets

  ```python
  data "vault_aws_access_credentials" "creds" {
    backend = data.terraform_remote_state.admin.outputs.backend
    role    = data.terraform_remote_state.admin.outputs.role
  }

  provider "aws" {
    region     = var.region
    access_key = data.vault_aws_access_credentials.creds.access_key
    secret_key = data.vault_aws_access_credentials.creds.secret_key
  }
  ```

### Variables

- [Doc](https://www.terraform.io/docs/language/values/variables.html)
- Using `TF_VAR_` env var for configs or secrets


### Dynamic Blocks

- [Doc](https://www.terraform.io/docs/language/expressions/dynamic-blocks.html)
- Example

  ```sh
  dynamic "setting" {
    for_each = var.settings
    content {
      namespace = setting.value["namespace"]
      name = setting.value["name"]
      value = setting.value["value"]
    }
  }
  ```

### Dependency Management

- `depends_on` (explicit dependency)
  - Only necessary when a resource or module relies on some other resource's behavior but _doesn't_ access any of that resource's data in its arguments (which is implicit dependency).
  - Available for `module` & `resource`.
- `terraform graph` for visualization

## Terraform Cloud & Enterprise

- Terraform Cloud
  - Hosted modules & providers on `app.terraform.io`
- [Feature Matrix](https://www.datocms-assets.com/2885/1602500234-terraform-full-feature-pricing-tablev2-1.pdf)
- [Sentinel](https://docs.hashicorp.com/sentinel): Policy as Code tool. It allows you to write policies to validate that your infrastructure is in its expected configuration.
- [Air Gap](https://www.hashicorp.com/blog/deploying-terraform-enterprise-in-airgapped-environments): Air Gap or disconnected network is a network security measure employed on one or more computers to ensure that a secure computer network is physically isolated from unsecured networks e.g. Public Internet
