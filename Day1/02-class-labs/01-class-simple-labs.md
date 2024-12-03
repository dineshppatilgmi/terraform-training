# **Tasks for Participants: Writing and Applying Terraform Configurations**

## **Task 1: Understand and Write a Basic Configuration**
**Objective:** Learn the basics of HCL by writing a Terraform configuration to create a simple resource in GCP.  

### 1. Create a directory for your Terraform project, e.g., `basic-terraform-project`.
### 2. Write a configuration file named `main.tf` with the following:
   - Provider: GCP
   - Resource: Create a Google Storage Bucket.
### 3. Example skeleton to follow:

```Bash
  provider "google" {  
     project = "your-project-id" # Replace this project id shared in session  
     region  = "us-central1"
   }  

  resource "google_storage_bucket" "trainingstorage" {  
      name  = "dinesh-patil-tf-trainning"  
      Iocation  = "us-central5"  
      force_destroy = true  
      public_access_prevention = "enforced"
  }

```

### 4. Run below commands to apply above configuration

```bash
terraform init

terraform fmt
## Check how your code formatted nicely after this command

terraform validate
## This command will check the code for syntax errors

terraform plan
## This will provide execution plan for your declared code

terraform apply
## This will actually go and apply your code.
```
> **WARNING**:  Check out what is problem with code above If you have noticed here `terraform validate` validated just syntax not the location value of your region in gcp. 

**Expected Outcome:** Bucket is created in the cloud.
---

## **Task 2: Modify and Reapply Configuration**
**Objective:** Modify the Terraform configuration and see how Terraform manages changes.  

### Demonstrate Addition of attributes in resources
**Add Configuration**: Change an attribute that modifies the existing resource without recreation.

```Bash
resource "google_storage_bucket" "trainingstorage" {  
    name  = "dinesh-patil-tf-training"  
    location  = "us-central5"  
    force_destroy = true  
    public_access_prevention = "enforced" 
    labels = {
        environment = "training"
        owner       = "dinesh-patil" # change this name don't leave it for my ownership...
    }
}

```

**Commands to Run:**

1. Run `terraform plan` to see how Terraform describes changes for addition of label. Look for lines marked as + indicating an add in place.

2. Run `terraform apply` to apply the changes.

**Expected Outcome:** 
- Terraform modifies the existing bucket instead of creating a new one.  
---

### Demonstrate Changes in Attributes
**Update Configuration**: Change a attribute that modifies the existing resource without recreation.

```Bash
resource "google_storage_bucket" "trainingstorage" {  
    name  = "dinesh-patil-tf-training"  
    location  = "us-central5"  
    force_destroy = true  
    public_access_prevention = "inherited" # Changed from "enforced"
}

```

**Commands to Run:**
1. Run `terraform plan` to see how Terraform describes changes in the public_access_prevention attribute. Look for lines marked as ~ indicating an update in place.
2. Run terraform apply to apply the changes.


### Demonstrate Changes that Require Resource Recreation
**Update Configuration**: Change an attribute that forces resource recreation.
```bash
resource "google_storage_bucket" "trainingstorage" {  
    name  = "dinesh-patil-tf-training"  
    location  = "us-east4"  # Changed from "us-central*"
    force_destroy = true  
    public_access_prevention = "enforced"  
}

```

**Commands to Run:**

1. Run terraform plan to observe that Terraform will mark the resource for recreation (- for destroy and + for create).
Example output:
```Bash
-/+ google_storage_bucket.trainingstorage
    location: "us-central5" => "us-east1" (forces new resource)
```
2. Run `terraform apply` to recreate the resource in the new location.



### **Summary of Tasks**
1. **Write a basic Terraform configuration file** using HCL.
2. **Run `terraform init`, `plan`, and `apply` commands** to provision resources in GCP.
3. **Modify the configuration** and observe how Terraform applies changes to manage the state.

---

### **Points to Highlight**

Terraform uses symbols in the plan to describe changes:  
* +: New resource creation.  
* -: Resource deletion.  
* ~: Inline attribute update.  
* -/+: Resource recreation (delete and re-create).  

#### The behavior of attributes depends on their immutability:
1. `Mutable` attributes can be updated in place (e.g., public_access_prevention).
2. `Immutable` attributes require resource recreation (e.g., location).

