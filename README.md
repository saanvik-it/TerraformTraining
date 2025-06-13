<div align="center">
  <img src="saanvikit.png" alt="Saanvik IT Logo" width="600"/>
  <h1>Azure Terraform Interview Questions and Answers</h1>
  <p><em>A comprehensive guide to prepare for Azure Terraform technical interviews</em></p>
  <p><strong>Website:</strong> <a href="https://saanvikit.com">https://saanvikit.com</a> | <strong>Contact:</strong> +91 9513184144</p>
  <hr>
</div>

## Table of Contents
- [Terraform Fundamentals](#terraform-fundamentals) (Questions 1-10)
- [Terraform State Management](#terraform-state-management) (Questions 11-13)
- [Terraform Resources and Configuration](#terraform-resources-and-configuration) (Questions 14-17)
- [Terraform Advanced Concepts](#terraform-advanced-concepts) (Questions 18-25)
- [Terraform Workflow and Commands](#terraform-workflow-and-commands) (Questions 26-34)

<hr>

## 🏗️ Terraform Fundamentals

### 1. 🤔 What is Terraform and why do we use it?
Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to define and provision infrastructure resources using a declarative configuration language called HashiCorp Configuration Language (HCL).

We use Terraform for several key reasons:
- **Infrastructure as Code**: Define infrastructure in code rather than through manual processes
- **Multi-cloud support**: Works with multiple cloud providers (AWS, Azure, GCP) and services
- **Declarative approach**: You specify the desired end state, not the steps to get there
- **State management**: Tracks the current state of your infrastructure
- **Execution plans**: Preview changes before applying them
- **Resource graph**: Creates a dependency graph for parallel resource creation
- **Modularity**: Reuse code through modules for common infrastructure patterns
- **Version control**: Infrastructure code can be versioned, shared, and collaborated on

### 2. 🧱 What are the building blocks of Terraform?
The main building blocks of Terraform include:

- **Providers**: Plugins that interact with cloud providers, SaaS providers, or other APIs
- **Resources**: Infrastructure objects managed by Terraform (VMs, networks, etc.)
- **Data Sources**: Read-only information fetched from providers about existing resources
- **Variables**: Input parameters for reusable configurations
- **Outputs**: Return values from resources that can be used by other configurations
- **Modules**: Containers for multiple resources that are used together
- **State**: Mapping of resources in your configuration to real-world resources
- **Backend**: Where and how the state is stored
- **Provisioners**: Tools to execute scripts or commands on resources after creation
- **Functions**: Built-in functions for transforming and combining values

### 3. ♻️ What is the lifecycle of Terraform?
The Terraform lifecycle consists of these main phases:

1. **Write**: Author your Terraform configuration files (.tf files)
2. **Initialize**: Run `terraform init` to download providers and modules
3. **Plan**: Run `terraform plan` to preview changes before applying
4. **Apply**: Run `terraform apply` to create or modify infrastructure
5. **Destroy**: Run `terraform destroy` when infrastructure is no longer needed

This lifecycle follows an "infrastructure as code" workflow where you:
- Define your infrastructure in code
- Version control your configurations
- Review changes before applying them
- Apply changes in a controlled manner
- Track the state of your infrastructure

### 4. 🖥️ What are the commands used to deploy the Terraform templates?
The main commands used to deploy Terraform templates are:

- `terraform init`: Initialize a Terraform working directory
- `terraform validate`: Validate the syntax of the Terraform files
- `terraform plan`: Create an execution plan
- `terraform apply`: Apply the changes required to reach the desired state
- `terraform destroy`: Destroy the Terraform-managed infrastructure

Additional useful commands include:
- `terraform fmt`: Format your configuration files
- `terraform show`: Inspect the current state
- `terraform output`: Extract output values from the state
- `terraform import`: Import existing infrastructure into Terraform
- `terraform state`: Advanced state management

### 5. 🔄 Can you briefly explain what happens when you execute each command?

- **terraform init**:
  - Initializes the working directory
  - Downloads and installs providers defined in configuration
  - Sets up the backend for storing state
  - Downloads modules referenced in configuration

- **terraform validate**:
  - Checks syntax of Terraform files
  - Verifies whether a configuration is syntactically valid and internally consistent
  - Does not access any remote services or state

- **terraform plan**:
  - Reads the current state
  - Compares it with the desired state in your configuration
  - Proposes an execution plan showing what Terraform will do
  - Shows additions, changes, and deletions without making any changes

- **terraform apply**:
  - By default, shows the execution plan and asks for confirmation
  - Creates, updates, or deletes infrastructure resources to match configuration
  - Updates the state file with the new state of resources
  - Outputs any defined output variables

- **terraform destroy**:
  - Creates a plan to delete all resources known to Terraform
  - After confirmation, removes all resources and updates the state file
  - Essentially runs `terraform apply` with a plan that deletes everything

### 6. 🔍 What is the difference between terraform validate vs terraform plan?

**terraform validate**:
- Only checks the syntax and validity of the configuration files
- Validates references between resources and variables
- Does not connect to any providers or check existing infrastructure
- Does not require initialized providers or backend
- Fast operation that only examines the configuration files
- Does not show what changes would be made

**terraform plan**:
- Connects to providers to check the current state of resources
- Compares the current state with the desired state in configuration
- Shows a detailed execution plan of what changes would be made
- Requires initialized providers and access to the state
- More comprehensive but slower than validate
- Can detect issues that validate cannot, like resource conflicts

### 7. 📄 What is terraform.tfstate and why do we need it?
The terraform.tfstate file is a JSON file that stores the state of your infrastructure. It maps the resources in your configuration to the real-world resources they represent.

We need the state file for several critical reasons:
- **Resource tracking**: Maps configuration to real-world resources
- **Metadata storage**: Stores resource dependencies and metadata
- **Performance optimization**: Caches resource attributes to avoid unnecessary API calls
- **Collaboration**: Enables team collaboration on the same infrastructure
- **Change detection**: Helps Terraform determine what needs to be created, updated, or deleted
- **Resource dependencies**: Tracks dependencies between resources for proper ordering of operations

Without the state file, Terraform would not know which real-world resources correspond to your configuration, potentially leading to duplicate resource creation or inability to manage existing resources.

### 8. 🗄️ Where do you store the statefile?
The state file can be stored in various locations, depending on your needs:

- **Local storage**: By default, Terraform stores the state file locally as `terraform.tfstate`
- **Remote backends**: For team environments, state should be stored remotely in:
  - **Azure Storage Account**: For Azure-focused deployments
  - **AWS S3**: With optional DynamoDB for locking
  - **Google Cloud Storage**: For GCP environments
  - **HashiCorp Terraform Cloud/Enterprise**: Managed service with additional features
  - **Consul**: HashiCorp's service networking platform
  - **PostgreSQL/MySQL**: Database backends
  - **HTTP**: Custom HTTP endpoint

Remote backends are preferred for team environments as they provide:
- Shared access to state
- State locking to prevent concurrent operations
- Secret encryption
- Versioning and backup capabilities

### 9. 🔒 How do you secure the statefile?
Securing the state file is critical as it may contain sensitive information:

- **Use remote backends** with access controls (Azure Storage with SAS tokens, S3 with IAM)
- **Enable encryption at rest** for the backend storage
- **Implement state locking** to prevent concurrent modifications
- **Use HTTPS** for all remote backend communications
- **Restrict access** using IAM/RBAC permissions to the minimum necessary users
- **Enable versioning** on the backend to recover from accidental changes
- **Audit access** to the state storage
- **Use sensitive output variables** to prevent sensitive values from appearing in logs
- **Consider using Terraform Cloud** which has built-in security features
- **Exclude sensitive data** from state when possible using the `sensitive` attribute

### 10. 🔄 If the statefile is deleted, how do you recover?
If the state file is deleted, recovery options include:

1. **Use state backups**: If you've enabled versioning on your remote backend, restore from a previous version
2. **Import existing resources**: Use `terraform import` to re-import each resource into a new state file
3. **Terraform refresh**: If you have a partial state, `terraform refresh` might help recover some information
4. **Recreation**: As a last resort, you might need to recreate resources and accept some downtime
5. **Terraform state pull/push**: If you have a local copy of the state, you can push it back to the remote backend

Best practices to avoid this situation:
- Always use remote backends with versioning enabled
- Implement proper access controls to prevent accidental deletion
- Regularly back up your state file
- Document your infrastructure outside of Terraform
- Consider using Terraform Cloud which provides additional state management features

## 💾 Terraform State Management

### 11. ⏱️ On which stage/command the statefile is generated?
The state file is initially generated during the first `terraform apply` command. Here's the process:

1. When you run `terraform init`, Terraform sets up the backend configuration but doesn't create a state file yet
2. During the first `terraform plan`, Terraform checks if a state file exists but doesn't create one
3. When you run `terraform apply` for the first time, Terraform:
   - Creates the resources defined in your configuration
   - Generates the state file to track those resources
   - Stores the state file in the configured backend

If you're using a remote backend, the state file is created in that remote location during the first apply. For subsequent operations, Terraform will read from and write to this existing state file.

### 12. 🔙 What is Terraform backend?
A Terraform backend defines where and how the state file is stored. It determines:

- Where state is stored
- How operations are performed
- How state locking occurs (if supported)

Common backend types include:

- **local**: Stores state on the local filesystem (default)
- **remote**: Stores state in Terraform Cloud
- **azurerm**: Stores state in Azure Blob Storage
- **s3**: Stores state in AWS S3 with optional locking via DynamoDB
- **gcs**: Stores state in Google Cloud Storage
- **http**: Stores state using REST client
- **consul**: Stores state in HashiCorp Consul

Backend configuration example for Azure:
```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "tfstatestorage"
    container_name       = "tfstate"
    key                  = "terraform.tfstate"
  }
}
```

Benefits of remote backends:
- Team collaboration
- State locking to prevent concurrent operations
- Secure storage of sensitive data
- Versioning and backup capabilities

### 13. 🔄 What exactly happens when you do terraform plan and apply?

**terraform plan**:
1. Reads the current state from the configured backend
2. Refreshes the state by querying the providers for the current status of managed resources
3. Compares the refreshed state with the desired state defined in your configuration
4. Builds a dependency graph of resources
5. Determines what changes are needed to achieve the desired state
6. Presents an execution plan showing what actions will be taken (create, update, delete)
7. Does not make any actual changes to infrastructure

**terraform apply**:
1. By default, runs a plan first and shows the execution plan
2. Asks for confirmation (unless using `-auto-approve`)
3. Upon confirmation, executes the plan:
   - Creates, updates, or deletes resources as needed
   - Handles resource dependencies in the correct order
   - Collects the current attributes of all resources
4. Updates the state file with the new state of all resources
5. Stores the updated state in the configured backend
6. Displays any output values defined in the configuration

The key difference is that `plan` only shows what would happen, while `apply` actually makes the changes to your infrastructure and updates the state file.

## 🧩 Terraform Resources and Configuration

### 14. 💻 What is the resource type used to deploy the Azure VM?
The primary resource type for deploying an Azure VM in Terraform is `azurerm_virtual_machine` (for older configurations) or the newer `azurerm_linux_virtual_machine` and `azurerm_windows_virtual_machine` resources.

Example of a basic Azure Linux VM deployment:
```hcl
resource "azurerm_linux_virtual_machine" "example" {
  name                = "example-vm"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  size                = "Standard_F2"
  admin_username      = "adminuser"
  
  network_interface_ids = [
    azurerm_network_interface.example.id,
  ]

  admin_ssh_key {
    username   = "adminuser"
    public_key = file("~/.ssh/id_rsa.pub")
  }

  os_disk {
    caching              = "ReadWrite"
    storage_account_type = "Standard_LRS"
  }

  source_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }
}
```

### 15. 📊 What are the data blocks/datasources?
Data sources in Terraform allow you to fetch or compute data for use elsewhere in your configuration. Unlike resources, data sources only read information and don't create or modify any infrastructure.

Data sources are defined using `data` blocks and are useful for:
- Referencing existing infrastructure not managed by Terraform
- Querying provider APIs for information
- Performing computations

Example of an Azure data source:
```hcl
data "azurerm_resource_group" "example" {
  name = "existing-resource-group"
}

resource "azurerm_virtual_network" "example" {
  name                = "example-network"
  location            = data.azurerm_resource_group.example.location
  resource_group_name = data.azurerm_resource_group.example.name
  address_space       = ["10.0.0.0/16"]
}
```

Common Azure data sources include:
- `azurerm_subscription`: Get information about the current subscription
- `azurerm_resource_group`: Get details of an existing resource group
- `azurerm_virtual_network`: Reference an existing virtual network
- `azurerm_subnet`: Reference an existing subnet
- `azurerm_image`: Find an existing VM image
- `azurerm_key_vault_secret`: Retrieve a secret from Azure Key Vault

### 16. 🔗 How do you refer the existing Azure resources in your Terraform configuration?
You can reference existing Azure resources in your Terraform configuration using:

1. **Data sources**: To query and use attributes of existing resources
   ```hcl
   data "azurerm_virtual_network" "existing" {
     name                = "production-network"
     resource_group_name = "networking-rg"
   }
   
   resource "azurerm_subnet" "example" {
     name                 = "example-subnet"
     resource_group_name  = data.azurerm_virtual_network.existing.resource_group_name
     virtual_network_name = data.azurerm_virtual_network.existing.name
     address_prefixes     = ["10.0.1.0/24"]
   }
   ```

2. **Terraform import**: To bring existing resources under Terraform management
   ```bash
   terraform import azurerm_resource_group.example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/example
   ```

3. **Remote state data source**: To reference resources managed by another Terraform configuration
   ```hcl
   data "terraform_remote_state" "network" {
     backend = "azurerm"
     config = {
       resource_group_name  = "tfstate-rg"
       storage_account_name = "tfstatestorage"
       container_name       = "tfstate"
       key                  = "network/terraform.tfstate"
     }
   }
   
   resource "azurerm_virtual_machine" "example" {
     # Reference a subnet output from the remote state
     subnet_id = data.terraform_remote_state.network.outputs.subnet_id
     # Other configuration...
   }
   ```

### 17. 📦 What are the Terraform modules?
Terraform modules are containers for multiple resources that are used together. They allow you to create reusable, shareable components to organize and encapsulate your infrastructure code.

Key aspects of Terraform modules:
- **Reusability**: Write code once and use it multiple times
- **Encapsulation**: Hide complex implementation details
- **Consistency**: Ensure resources are deployed consistently
- **Composition**: Build larger systems from smaller components
- **Versioning**: Track and control changes to infrastructure components

A module typically includes:
- Input variables to customize behavior
- Resources to be provisioned
- Output values to expose information

Example of a module structure:
```
modules/
└── virtual_machine/
    ├── main.tf         # Contains the main resource definitions
    ├── variables.tf    # Input variables declaration
    ├── outputs.tf      # Output values
    └── README.md       # Documentation
```

Example of using a module:
```hcl
module "web_server" {
  source              = "./modules/virtual_machine"
  name                = "web-server"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  vm_size             = "Standard_D2s_v3"
  subnet_id           = azurerm_subnet.example.id
}
```

Modules can be sourced from:
- Local paths
- Terraform Registry (public or private)
- Git repositories
- HTTP URLs
- S3 buckets and other storage systems
## 🚀 Terraform Advanced Concepts

### 18. 🛠️ What are common challenges/issues that you have faced while working with Terraform?
Common challenges when working with Terraform include:

1. **State management issues**:
   - State file corruption or loss
   - Concurrent state modifications causing conflicts
   - Large state files causing performance issues

2. **Dependency management**:
   - Implicit dependencies not being detected
   - Circular dependencies between resources
   - Managing dependencies across multiple modules

3. **Version compatibility**:
   - Breaking changes between Terraform versions
   - Provider version compatibility issues
   - Module version constraints

4. **Performance challenges**:
   - Slow plan/apply operations with large infrastructures
   - API rate limiting from cloud providers
   - Resource timeouts during creation or deletion

5. **Secret management**:
   - Securely handling credentials and sensitive data
   - Preventing secrets from being stored in state
   - Managing different credentials across environments

6. **Team collaboration**:
   - Coordinating changes across team members
   - Implementing proper CI/CD for infrastructure
   - Maintaining consistent coding standards

7. **Refactoring challenges**:
   - Renaming or moving resources without recreation
   - Splitting large configurations into modules
   - Managing state during major refactoring

8. **Error handling and debugging**:
   - Cryptic error messages from providers
   - Difficulty troubleshooting failed deployments
   - Limited visibility into provider API calls

Solutions typically involve:
- Using remote backends with state locking
- Implementing proper CI/CD pipelines
- Breaking configurations into smaller, manageable modules
- Using consistent versioning strategies
- Implementing automated testing for infrastructure
- Using external secret management solutions

### 19. 🧭 What is Terraform drift? How do you detect it?
Terraform drift occurs when the actual state of your infrastructure differs from what is defined in your Terraform configuration or stored in the state file. This can happen when:
- Resources are modified outside of Terraform (via console, CLI, etc.)
- Manual emergency changes are made
- Other automation tools modify the infrastructure
- Resources are deleted manually

To detect drift, you can use:

1. **terraform plan**: This command will show differences between the current state and desired configuration
   ```bash
   terraform plan
   ```
   If there are differences, the plan will show what changes would be made to bring the infrastructure back to the desired state.

2. **terraform refresh**: Updates the state file to match the real-world infrastructure
   ```bash
   terraform refresh
   ```

3. **Custom drift detection tools**: Tools like driftctl, Terragrunt, or cloud provider-specific tools

4. **Automated drift detection**: Set up scheduled jobs to run plan and alert on differences

To remediate drift:
- Run `terraform apply` to bring infrastructure back to the desired state
- Update your Terraform configuration to match intentional manual changes
- Implement policies to prevent manual changes to Terraform-managed resources
- Use resource locks where appropriate

### 20. 👻 What are Terraform null resources?
The `null_resource` is a special resource type in Terraform that doesn't correspond to any real infrastructure object. It's primarily used for:

1. **Running provisioners not directly associated with a resource**:
   ```hcl
   resource "null_resource" "example" {
     provisioner "local-exec" {
       command = "echo 'Hello, World!'"
     }
   }
   ```

2. **Creating dependencies between resources that don't have explicit relationships**:
   ```hcl
   resource "null_resource" "dependency" {
     depends_on = [
       azurerm_virtual_machine.example,
       azurerm_sql_database.example
     ]
   
     provisioner "local-exec" {
       command = "echo 'All dependencies are created!'"
     }
   }
   ```

3. **Triggering actions based on changes to values**:
   ```hcl
   resource "null_resource" "trigger_example" {
     # This will cause the null_resource to be recreated when the value changes
     triggers = {
       cluster_instance_ids = join(",", aws_instance.cluster.*.id)
     }
   
     provisioner "local-exec" {
       command = "echo 'Cluster configuration changed!'"
     }
   }
   ```

4. **Implementing custom logic or workflows**:
   ```hcl
   resource "null_resource" "database_setup" {
     depends_on = [azurerm_sql_server.example]
   
     provisioner "local-exec" {
       command = "python setup_database.py --server ${azurerm_sql_server.example.fqdn}"
     }
   }
   ```

The `null_resource` implements the standard resource lifecycle but takes no other actions.

### 21. 🔄 What is Terraform dynamic block and why do we use it?
A dynamic block in Terraform allows you to dynamically create multiple nested blocks within a resource or module block based on a collection (like a list or map). This is particularly useful for creating repeated nested blocks without duplicating code.

Dynamic blocks are commonly used for:
- Security group rules
- Ingress/egress rules
- IAM policy statements
- Route tables
- Any resource with repeatable nested blocks

Example of a dynamic block for network security rules:
```hcl
variable "security_rules" {
  type = list(object({
    name                       = string
    priority                   = number
    direction                  = string
    access                     = string
    protocol                   = string
    source_port_range          = string
    destination_port_range     = string
    source_address_prefix      = string
    destination_address_prefix = string
  }))
  default = [
    {
      name                       = "HTTP"
      priority                   = 100
      direction                  = "Inbound"
      access                     = "Allow"
      protocol                   = "Tcp"
      source_port_range          = "*"
      destination_port_range     = "80"
      source_address_prefix      = "*"
      destination_address_prefix = "*"
    },
    {
      name                       = "HTTPS"
      priority                   = 101
      direction                  = "Inbound"
      access                     = "Allow"
      protocol                   = "Tcp"
      source_port_range          = "*"
      destination_port_range     = "443"
      source_address_prefix      = "*"
      destination_address_prefix = "*"
    }
  ]
}

resource "azurerm_network_security_group" "example" {
  name                = "example-nsg"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  dynamic "security_rule" {
    for_each = var.security_rules
    content {
      name                       = security_rule.value.name
      priority                   = security_rule.value.priority
      direction                  = security_rule.value.direction
      access                     = security_rule.value.access
      protocol                   = security_rule.value.protocol
      source_port_range          = security_rule.value.source_port_range
      destination_port_range     = security_rule.value.destination_port_range
      source_address_prefix      = security_rule.value.source_address_prefix
      destination_address_prefix = security_rule.value.destination_address_prefix
    }
  }
}
```

Benefits of dynamic blocks:
- Reduces code duplication
- Makes configurations more maintainable
- Allows for parameterization of repeated blocks
- Simplifies complex resource configurations

### 22. 🔢 Which Terraform version you have used?
In a real interview, you would mention the specific versions you've worked with. A good answer might be:

"I've worked with multiple Terraform versions throughout my career, most recently using Terraform 1.5.x and 1.6.x in production environments. I've also maintained legacy code that used versions as old as 0.12.x and managed the upgrade process to newer versions. I stay current with HashiCorp's release notes to understand new features, deprecations, and breaking changes between versions."

Key Terraform version milestones:
- 0.12: Introduced significant HCL improvements
- 0.13: Added module count and for_each support
- 0.14: Improved state locking and added sensitive variable feature
- 0.15: Provider dependency lockfile
- 1.0: First stable release with long-term support
- 1.1: Added custom condition checks and preconditions
- 1.2: Added support for testing experiments
- 1.3: Improved moved block functionality
- 1.4: Enhanced import functionality
- 1.5: Added testing framework and check blocks
- 1.6: Improved validation features and testing capabilities

### 23. 🔌 Which Terraform provider version you have used?
For Azure deployments, a typical answer might be:

"For Azure deployments, I've primarily used the AzureRM provider versions 3.x, specifically 3.0 through 3.75. I've also worked with the AzureAD provider (now Microsoft Graph provider) for identity management. When working with multiple providers, I use version constraints in the required_providers block to ensure compatibility and predictable behavior."

Example of provider version configuration:
```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.75.0"
    }
    azuread = {
      source  = "hashicorp/azuread"
      version = "~> 2.45.0"
    }
  }
  required_version = ">= 1.5.0"
}
```

### 24. ☁️ What is the Terraform provider for Azure?
The primary Terraform provider for Azure is the **AzureRM provider** (`hashicorp/azurerm`). This provider allows Terraform to interact with the many resources supported by Azure Resource Manager.

Additional Azure-related providers include:
- **AzureAD** (now Microsoft Graph): For managing Azure Active Directory resources
- **AzureDevOps**: For managing Azure DevOps resources
- **AzAPI**: For managing Azure resources not yet supported in the AzureRM provider

To configure the AzureRM provider:
```hcl
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.75.0"
    }
  }
}

provider "azurerm" {
  features {}
  # Authentication can be provided via:
  # - Azure CLI login
  # - Service Principal with Client Certificate
  # - Service Principal with Client Secret
  # - Managed Service Identity
}
```

Authentication methods for the AzureRM provider:
1. **Azure CLI**: Uses your existing Azure CLI login
2. **Service Principal with Client Secret**:
   ```hcl
   provider "azurerm" {
     features {}
     subscription_id = "00000000-0000-0000-0000-000000000000"
     client_id       = "00000000-0000-0000-0000-000000000000"
     client_secret   = var.client_secret
     tenant_id       = "00000000-0000-0000-0000-000000000000"
   }
   ```
3. **Service Principal with Certificate**
4. **Managed Identity**: When running from an Azure resource with managed identity

### 25. 🖥️ When you create a VM, what are all the resource type configurations needed in Terraform?
To create a complete Azure VM deployment in Terraform, you typically need the following resources:

1. **Resource Group**:
   ```hcl
   resource "azurerm_resource_group" "example" {
     name     = "example-resources"
     location = "East US"
   }
   ```

2. **Virtual Network and Subnet**:
   ```hcl
   resource "azurerm_virtual_network" "example" {
     name                = "example-network"
     address_space       = ["10.0.0.0/16"]
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name
   }

   resource "azurerm_subnet" "example" {
     name                 = "internal"
     resource_group_name  = azurerm_resource_group.example.name
     virtual_network_name = azurerm_virtual_network.example.name
     address_prefixes     = ["10.0.2.0/24"]
   }
   ```

3. **Network Interface**:
   ```hcl
   resource "azurerm_network_interface" "example" {
     name                = "example-nic"
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name

     ip_configuration {
       name                          = "internal"
       subnet_id                     = azurerm_subnet.example.id
       private_ip_address_allocation = "Dynamic"
     }
   }
   ```

4. **Public IP Address** (optional):
   ```hcl
   resource "azurerm_public_ip" "example" {
     name                = "example-pip"
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name
     allocation_method   = "Dynamic"
   }
   ```

5. **Network Security Group** (optional but recommended):
   ```hcl
   resource "azurerm_network_security_group" "example" {
     name                = "example-nsg"
     location            = azurerm_resource_group.example.location
     resource_group_name = azurerm_resource_group.example.name

     security_rule {
       name                       = "SSH"
       priority                   = 1001
       direction                  = "Inbound"
       access                     = "Allow"
       protocol                   = "Tcp"
       source_port_range          = "*"
       destination_port_range     = "22"
       source_address_prefix      = "*"
       destination_address_prefix = "*"
     }
   }
   ```

6. **Virtual Machine**:
   ```hcl
   resource "azurerm_linux_virtual_machine" "example" {
     name                = "example-machine"
     resource_group_name = azurerm_resource_group.example.name
     location            = azurerm_resource_group.example.location
     size                = "Standard_F2"
     admin_username      = "adminuser"
     network_interface_ids = [
       azurerm_network_interface.example.id,
     ]

     admin_ssh_key {
       username   = "adminuser"
       public_key = file("~/.ssh/id_rsa.pub")
     }

     os_disk {
       caching              = "ReadWrite"
       storage_account_type = "Standard_LRS"
     }

     source_image_reference {
       publisher = "Canonical"
       offer     = "UbuntuServer"
       sku       = "18.04-LTS"
       version   = "latest"
     }
   }
   ```

7. **Managed Disk** (optional additional disks):
   ```hcl
   resource "azurerm_managed_disk" "example" {
     name                 = "example-disk"
     location             = azurerm_resource_group.example.location
     resource_group_name  = azurerm_resource_group.example.name
     storage_account_type = "Standard_LRS"
     create_option        = "Empty"
     disk_size_gb         = 100
   }

   resource "azurerm_virtual_machine_data_disk_attachment" "example" {
     managed_disk_id    = azurerm_managed_disk.example.id
     virtual_machine_id = azurerm_linux_virtual_machine.example.id
     lun                = "10"
     caching            = "ReadWrite"
   }
   ```

8. **Boot Diagnostics** (optional):
   ```hcl
   resource "azurerm_storage_account" "example" {
     name                     = "examplediagnostics"
     resource_group_name      = azurerm_resource_group.example.name
     location                 = azurerm_resource_group.example.location
     account_tier             = "Standard"
     account_replication_type = "LRS"
   }
   ```

   Then reference in the VM:
   ```hcl
   resource "azurerm_linux_virtual_machine" "example" {
     # ... other configuration ...
     
     boot_diagnostics {
       storage_account_uri = azurerm_storage_account.example.primary_blob_endpoint
     }
   }
   ```
## ⚙️ Terraform Workflow and Commands

### 26. 🛠️ What are Terraform provisioners?
Terraform provisioners are used to execute scripts or commands on a local machine or on a remote resource after it's created. They allow you to bootstrap instances, configure applications, or perform other setup tasks that aren't directly supported by Terraform's declarative model.

Types of provisioners:
- **file**: Copies files from the machine running Terraform to the newly created resource
- **local-exec**: Executes a command on the machine running Terraform
- **remote-exec**: Executes a command on the newly created resource

Example of using provisioners:
```hcl
resource "azurerm_linux_virtual_machine" "example" {
  # ... VM configuration ...

  # Copy a configuration file to the VM
  provisioner "file" {
    source      = "app.conf"
    destination = "/etc/app.conf"

    connection {
      type        = "ssh"
      user        = "adminuser"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip_address
    }
  }

  # Execute commands on the VM
  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]

    connection {
      type        = "ssh"
      user        = "adminuser"
      private_key = file("~/.ssh/id_rsa")
      host        = self.public_ip_address
    }
  }

  # Execute a local script after VM creation
  provisioner "local-exec" {
    command = "echo ${self.public_ip_address} >> vm_ips.txt"
  }
}
```

Important considerations:
- Provisioners are a last resort and should be avoided when possible
- They can make your Terraform code less predictable and harder to maintain
- They only run during creation or destruction, not during updates
- If a provisioner fails, the resource is still marked as created
- Consider using dedicated configuration management tools instead (Ansible, Chef, Puppet)

### 27. 🧰 How many types of Terraform provisioners we have? Give some examples.
Terraform has two main categories of provisioners:

**1. Creation-Time Provisioners** (default):
- Run when a resource is created
- Do not run during update or destroy operations
- If they fail, the resource is marked as tainted

**2. Destroy-Time Provisioners**:
- Run before a resource is destroyed
- Specified using `when = destroy` attribute
- Useful for cleanup operations

Specific provisioner types:

**file provisioner**:
```hcl
provisioner "file" {
  source      = "local/path/to/file.txt"
  destination = "/remote/path/file.txt"
  
  connection {
    type     = "ssh"
    user     = "adminuser"
    password = var.admin_password
    host     = azurerm_public_ip.example.ip_address
  }
}
```

**local-exec provisioner**:
```hcl
provisioner "local-exec" {
  command = "echo The server's IP address is ${self.private_ip}"
  
  # Optional attributes
  working_dir = "~/scripts"
  interpreter = ["/bin/bash", "-c"]
  environment = {
    FOO = "bar"
  }
}
```

**remote-exec provisioner**:
```hcl
provisioner "remote-exec" {
  inline = [
    "sudo apt-get update",
    "sudo apt-get install -y nginx",
    "sudo systemctl enable nginx"
  ]
  
  connection {
    type        = "ssh"
    user        = "adminuser"
    private_key = file("~/.ssh/id_rsa")
    host        = self.public_ip_address
  }
}
```

**chef provisioner**:
```hcl
provisioner "chef" {
  server_url      = "https://chef.example.com/organizations/org1"
  node_name       = "webserver1"
  run_list        = ["role[web]"]
  secret_key      = file("~/chef/encrypted_data_bag_secret")
  version         = "12.22.5"
}
```

**habitat provisioner**:
```hcl
provisioner "habitat" {
  service_name    = "core/redis"
  peer           = "1.2.3.4"
}
```

**salt-masterless provisioner**:
```hcl
provisioner "salt-masterless" {
  local_state_tree = "/srv/salt"
  remote_state_tree = "/srv/salt"
}
```

### 28. 🔀 What are Terraform workspaces?
Terraform workspaces allow you to manage multiple distinct sets of infrastructure resources using the same Terraform configuration. Each workspace has its own state file, allowing you to maintain different environments (like development, staging, and production) with the same code.

Key workspace commands:
- `terraform workspace list`: List available workspaces
- `terraform workspace new <name>`: Create a new workspace
- `terraform workspace select <name>`: Switch to a different workspace
- `terraform workspace show`: Show the current workspace
- `terraform workspace delete <name>`: Delete a workspace

Example of using workspaces:
```hcl
resource "azurerm_resource_group" "example" {
  name     = "rg-${terraform.workspace}"
  location = "East US"
}

resource "azurerm_virtual_network" "example" {
  name                = "vnet-${terraform.workspace}"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  address_space       = ["10.0.0.0/16"]
}
```

You can also use workspaces for conditional configuration:
```hcl
locals {
  environment_config = {
    default = {
      vm_size = "Standard_D2s_v3"
      instance_count = 1
    }
    development = {
      vm_size = "Standard_B2s"
      instance_count = 1
    }
    production = {
      vm_size = "Standard_D4s_v3"
      instance_count = 3
    }
  }

  # Use the current workspace, or default if the workspace doesn't have specific config
  config = lookup(local.environment_config, terraform.workspace, local.environment_config.default)
}

resource "azurerm_linux_virtual_machine" "example" {
  count               = local.config.instance_count
  name                = "vm-${terraform.workspace}-${count.index}"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  size                = local.config.vm_size
  # ... other configuration ...
}
```

Limitations of workspaces:
- All workspaces share the same backend configuration
- Not ideal for completely separate configurations
- Can become complex to manage with many environments
- Not a replacement for proper module parameterization

### 29. 🎯 What is Terraform target?
The `-target` option in Terraform allows you to focus operations on a specific resource or module. This is useful for:
- Speeding up development cycles by applying only relevant resources
- Fixing specific resources without affecting others
- Testing changes to a subset of your infrastructure

Example usage:
```bash
# Apply changes only to a specific resource
terraform apply -target=azurerm_virtual_machine.example

# Apply changes to all resources in a module
terraform apply -target=module.networking

# Destroy a specific resource
terraform destroy -target=azurerm_resource_group.example
```

Important considerations:
- `-target` is not recommended for regular use in production
- It can break the dependency graph and leave your infrastructure in an inconsistent state
- It's primarily intended for troubleshooting and development
- HashiCorp recommends using it sparingly

### 30. 🔄 What is the function we use to deploy multiple resources in Terraform?
Terraform provides two main meta-arguments for creating multiple instances of a resource:

1. **count**: Creates multiple instances based on a number
   ```hcl
   resource "azurerm_virtual_machine" "example" {
     count               = 3
     name                = "vm-${count.index}"
     resource_group_name = azurerm_resource_group.example.name
     location            = azurerm_resource_group.example.location
     # ... other configuration ...
   }
   ```

2. **for_each**: Creates multiple instances based on a map or set
   ```hcl
   # Using a map
   resource "azurerm_virtual_machine" "example" {
     for_each            = {
       web = { size = "Standard_D2s_v3", zone = "1" }
       app = { size = "Standard_D4s_v3", zone = "2" }
       db  = { size = "Standard_D8s_v3", zone = "3" }
     }
     name                = "vm-${each.key}"
     resource_group_name = azurerm_resource_group.example.name
     location            = azurerm_resource_group.example.location
     size                = each.value.size
     zone                = each.value.zone
     # ... other configuration ...
   }
   
   # Using a set
   resource "azurerm_virtual_machine" "example" {
     for_each            = toset(["web", "app", "db"])
     name                = "vm-${each.key}"
     resource_group_name = azurerm_resource_group.example.name
     location            = azurerm_resource_group.example.location
     # ... other configuration ...
   }
   ```

Additionally, you can use:
- **Dynamic blocks**: For creating multiple nested blocks within a resource
- **Modules with count or for_each**: For creating multiple instances of a module

### 31. 🔢 What is the difference between count.index vs for_each?

**count.index**:
- Creates multiple instances of a resource based on a numeric index
- Resources are identified by their index (0, 1, 2, etc.)
- Better for homogeneous resources where all instances are similar
- Changes to the count can cause resource recreation if items are removed from the middle
- Accessing resources: `azurerm_virtual_machine.example[0]`

**for_each**:
- Creates multiple instances based on a map or set
- Resources are identified by the map keys or set values
- Better for heterogeneous resources with different configurations
- More stable during changes as resources are tracked by their keys
- Accessing resources: `azurerm_virtual_machine.example["web"]`

Example showing the difference in behavior:

With `count`:
```hcl
# Initial configuration
resource "azurerm_virtual_machine" "example" {
  count = 3
  name  = "vm-${count.index}"
  # ... other configuration ...
}

# If you remove the middle element, all subsequent resources are recreated
# vm-0 stays the same
# vm-1 is destroyed
# vm-2 is destroyed and recreated as vm-1
```

With `for_each`:
```hcl
# Initial configuration
resource "azurerm_virtual_machine" "example" {
  for_each = toset(["web", "app", "db"])
  name     = "vm-${each.key}"
  # ... other configuration ...
}

# If you remove "app", only that resource is affected
# vm-web stays the same
# vm-app is destroyed
# vm-db stays the same
```

When to use each:
- Use `count` for simple scaling of identical resources
- Use `for_each` when resources have unique properties or when you need stable identifiers

### 32. 📝 What is the difference between variables.tf vs variables.tfvars?

**variables.tf**:
- Declares variables and their types
- Defines default values (optional)
- Provides descriptions and validation rules
- Part of the configuration code
- Should be committed to version control

Example `variables.tf`:
```hcl
variable "resource_group_name" {
  type        = string
  description = "Name of the resource group"
}

variable "location" {
  type        = string
  default     = "East US"
  description = "Azure region for resources"
}

variable "vm_sizes" {
  type        = map(string)
  default     = {
    small  = "Standard_B2s"
    medium = "Standard_D2s_v3"
    large  = "Standard_D4s_v3"
  }
  description = "Available VM sizes"
}
```

**terraform.tfvars** or custom `.tfvars` files:
- Assigns actual values to variables
- Contains environment-specific configurations
- May contain sensitive information
- Often excluded from version control
- Used to separate configuration from implementation

Example `terraform.tfvars`:
```hcl
resource_group_name = "production-rg"
location            = "West US 2"
vm_sizes = {
  small  = "Standard_B2ms"
  medium = "Standard_D2s_v3"
  large  = "Standard_D8s_v3"
}
```

How to use `.tfvars` files:
- Default files (`terraform.tfvars` or `*.auto.tfvars`) are loaded automatically
- Custom files can be specified with `-var-file` flag:
  ```bash
  terraform apply -var-file="production.tfvars"
  ```

Best practices:
- Use `variables.tf` to define the structure of your variables
- Use `.tfvars` files for environment-specific values
- Keep sensitive values in separate, gitignored `.tfvars` files
- Consider using a secrets manager for sensitive values in production

### 33. ⬇️ What is Terraform import?
Terraform import is a command that allows you to bring existing infrastructure resources under Terraform management. This is useful when:
- You have resources created manually or by other tools
- You're migrating from another IaC tool to Terraform
- You need to recover from a lost state file

Basic usage:
```bash
terraform import [options] ADDRESS ID
```

Example importing an Azure resource group:
```bash
terraform import azurerm_resource_group.example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/example
```

The process typically involves:
1. Writing a Terraform configuration that matches the existing resource
2. Running the `terraform import` command to import the resource into state
3. Running `terraform plan` to verify there are no differences
4. If there are differences, updating either the configuration or the resource

Example workflow:
```hcl
# 1. Create the resource configuration in your .tf file
resource "azurerm_resource_group" "example" {
  name     = "example"
  location = "East US"
  # Note: You need to know the exact configuration or Terraform will try to modify the resource
}

# 2. Import the resource
# terraform import azurerm_resource_group.example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/example

# 3. Run terraform plan to check for differences
# terraform plan
```

Limitations:
- You must create the resource configuration manually before importing
- Not all resources support import
- Complex resources may require multiple import commands
- Importing doesn't automatically generate configuration

### 34. 🔓 How do you unlock your statefile if it's in lock state?
When Terraform operations are running, they lock the state file to prevent concurrent modifications. Sometimes, if a Terraform process terminates unexpectedly, the lock might not be released properly.

To unlock a state file:

1. **For local state**:
   - Delete the `.terraform.tfstate.lock.info` file in your working directory

2. **For remote state in Azure Storage**:
   - Use the `terraform force-unlock` command with the lock ID:
     ```bash
     terraform force-unlock LOCK_ID
     ```
   - The lock ID can be found in the error message when Terraform fails due to a locked state

3. **For Terraform Cloud/Enterprise**:
   - Go to the run in the Terraform Cloud UI
   - Click "Force cancel run" to release the lock

4. **For other backends**:
   - Use backend-specific tools (e.g., AWS CLI for S3/DynamoDB)
   - Use the `terraform force-unlock` command

Example of force-unlock:
```bash
terraform force-unlock 55555555-5555-5555-5555-555555555555
```

Important considerations:
- Only use force-unlock when you're certain no other Terraform process is running
- Forcing an unlock when another process is running can corrupt your state
- Always try to determine why the lock wasn't released properly
- Consider implementing automated lock breaking in CI/CD pipelines with appropriate safeguards

Best practices:
- Set reasonable timeouts for operations to prevent abandoned locks
- Implement proper error handling in CI/CD pipelines
- Document the unlock procedure for your team
- Consider using managed backends like Terraform Cloud that have better lock management
