# **Tasks for Participants: State**

## **Class-Labs**

### **Task 1: Playing with State file** 

**Objective:** Understand the purpose and structure of the local state file.

Initialize Terraform and deploy a bucket to observe the terraform.tfstate file.

```Bash

resource "google_storage_bucket" "my-bucket" {
  name     = "dinesh-patil-terraform-bucket"
  location = "US"
}

```

Run Terraform apply command and then inspect the state file terraform.tfstate file


#### Try running below commands to check the resources in state

##### Description: Lists all resources in the Terraform state file.

```Bash
terraform state list
```

This will output all managed resources, such as google_storage_bucket.my-bucket.

##### Description: Displays detailed information about a specific resource in the state file.

```Bash
terraform state show <resource>
```

This will show the attributes of the specified bucket.

##### Description: Removes a resource from the state file without destroying the actual resource.

## **Note ** Take a backup of terraform state file before this activity.

```Bash
terraform state rm <resource>
```

This removes the resource from Terraform's management but keeps it in the cloud. 

Try running the terraform plan command check the result .

## Now replace the old terraformstatefile with the backup one.

Check if it shows the resources. `terraform state list`

### **Task 2: Remote State**

**Objective:** Configure remote state storage to collaborate securely. Store the state remotely using a GCS bucket.


```Bash
terraform {
  backend "gcs" {
    bucket  = "state-storage-bucket" # check with me,sagar,mithun,viraj for this name
    prefix  = "terraform/dinesh-patil/state"   
  }
}
```

go to google console and check how is the folder structure

#### Comment the backend configuration like below

```Bash
terraform {
#   backend "gcs" {
#     bucket  = "state-storage-bucket" # check with me,sagar,mithun,viraj for this name
#     prefix  = "terraform/dinesh-patil/state"   
#   }
}
```

check out the instrcution on the screen and run command about `terraform init -migrate-state`

Now you will be back to local config

### **Task 3: Creating Workspaces**

Objective: Understand how to create and manage Terraform workspaces.

Create workspaces for dev, staging, and prod.

```Bash

terraform workspace new dev
terraform workspace new staging

```

### **Task 4: Workspace-Specific Resources**
**Objective:** Use workspaces to configure environment-specific resources.

Use ${terraform.workspace} for environment-specific resource naming.

```Bash 

resource "google_storage_bucket" "example" {
  name     = "my-bucket-${terraform.workspace}"
  location = "US"
}

# run below commands
terraform workspace select dev
terrafrom apply --auto-approve

terraform workspace select staging
terrafrom apply --auto-approve

```

check how the state file is manage for different workspace