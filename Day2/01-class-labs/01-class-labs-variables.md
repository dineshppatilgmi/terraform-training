# **Tasks for Participants: Variables**

## **Class-Labs**

### **Task 1: Declaring Variables** 

**Objective:** Understand how to declare variables in Terraform.
  
Create variables for a Google Cloud Storage bucket's name and location in a `variables.tf` file.  

```Bash
variable "bucket_name" {
  default = "dinesh-patil-terraform-bucket"
}

variable "location" {
  defualt = "US-central1"
}
```

### **Task 2: Using Variables in Resources**
**Objective:** Learn how to use variables within resource definitions.

Use the declared variables to configure a Google Cloud Storage bucket.

```Bash

resource "google_storage_bucket" "example" {
  name     = var.bucket_name
  location = var.location
}
```

### **Task 3: Setting Variable Values**

**Objective:** Learn multiple ways to provide values to Terraform variables.

Pass variable values via a .tfvars file and the command line.

Create terraform.tfvars:
```Bash
bucket_name = "dinesh-patil-fromtfvars-bucket"
location    = "ASIA"
```

Check which value is coming in `terraform plan`

Pass values dynamically:

```Bash

terraform plan -var="bucket_name=dinesh-patil-fromcli-bucket" -var="location=EUROPE"
```

