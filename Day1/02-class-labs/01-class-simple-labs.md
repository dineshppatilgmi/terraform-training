### **Tasks for Participants: Writing and Applying Terraform Configurations**

#### **Task 1: Understand and Write a Basic Configuration**
**Objective:** Learn the basics of HCL by writing a Terraform configuration to create a simple resource in GCP.  

###### 1. Create a directory for your Terraform project, e.g., `basic-terraform-project`.
###### 2. Write a configuration file named `main.tf` with the following:
   - Provider: GCP
   - Resource: Create a Google Storage Bucket.
###### 3. Example skeleton to follow:
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

###### 4. Run below commands to apply above configuration
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

#### **Task 2: Modify and Reapply Configuration**
**Objective:** Modify the Terraform configuration and see how Terraform manages changes.  

##### Demonstrate Changes in Attributes
**Update Configuration**: Change an attribute that modifies the existing resource without recreation.

```Bash
resource "google_storage_bucket" "trainingstorage" {  
    name  = "dinesh-patil-tf-training"  
    location  = "us-central5"  
    force_destroy = true  
    public_access_prevention = "enforced" 
    labels = {
        environment = "training"
        owner       = "dinesh-patil" # change this name don't leave it for my ownership :stuck_out_tongue_winking_eye:
    }
}
```

Commands to Run:
Run terraform plan to see how Terraform describes changes in the public_access_prevention attribute. Look for lines marked as ~ indicating an update in place.
Run terraform apply to apply the changes.
**Expected Outcome:** 
- Terraform modifies the existing instance instead of creating a new one.  
- Students understand how Terraform manages the "desired state."

---

### **Summary of Tasks**
1. **Write a basic Terraform configuration file** using HCL.
2. **Run `terraform init`, `plan`, and `apply` commands** to provision resources in GCP.
3. **Modify the configuration** and observe how Terraform applies changes to manage the state.

---

#### **Home Extension**
- Add another resource (e.g., a GCP storage bucket) to your existing configuration and apply it.
- Experiment with destroying and recreating the infrastructure using:
  ```bash
  terraform destroy
  ```