# Terraform Variable Exploration Tasks


## Task 1: 
- **Question**: Make the GCP provider configuration more flexible by using variables for project ID and region (Use Seperate file for variables like variables.tf and main.tf for actual config).
<details>
<summary>Hint</summary>
Define variables for `project_id` and `region`, and reference them in the provider configuration.
</details>
<details>
<summary>Solution</summary>

```bash
# variables.tf
variable "project_id" {
  type = string
}

variable "region" {
  type = string
}

# main.tf
provider "google" {
  project = var.project_id
  region  = var.region
}
```
This sets the backend to use a specific state file path.
</details>

---

## Task 2: 
- **Question**: create a GCP Storage Bucket with a dynamic name using a variable? Use the random_id resource from random provider to add suffix to the bucket name using below requirement. 
  - take two variable firstname and lastname
  - bucketname should start with your firstname-lastname
  - convert random id to hex and pick only first 7 character of hex
  - add - before hex value
<details>
<summary>Hint</summary> 
Use the `random_id` resource to generate a random suffix, convert it to hexadecimal, and slice it to take only the first 7 characters. Concatenate this with your first and last name to form a unique bucket name. 
</details> 

<details> 
<summary>Solution</summary>

```bash
# variables.tf
variable "first_name" {
  type = string
  default = "John"  # Replace with your first name
}

variable "last_name" {
  type = string
  default = "Doe"   # Replace with your last name
}

# main.tf
resource "random_id" "bucket_suffix" {
  byte_length = 4
}

resource "google_storage_bucket" "bucket" {
  name     = "${var.first_name}-${var.last_name}-${substr(random_id.bucket_suffix.hex, 0, 7)}"
  location = var.region
}
```

</details>
</details>

---

## Task 3: 
- **Question**: How can you enable uniform bucket-level access using a boolean variable?
<details>
<summary>Hint</summary>
Define a `uniform_bucket_level_access` variable and set it to `true` or `false`. 
</details>

<details>
<summary>Solution</summary>

```bash
# variables.tf
variable "uniform_bucket_level_access" {
  type    = bool
  default = true
}

# main.tf
resource "google_storage_bucket" "bucket" {
  name                            = var.bucket_name
  location                        = var.region
  uniform_bucket_level_access     = var.uniform_bucket_level_access
}

```
</details>

---

## Task 4:  
- **Question:** Create a terraform config which will accept input for storage class by variable `storage-class` and configure storage_class (which should not accept the class `NEARLINE`,`COLDLINE`,`ARCHIVE`,`MULTI_REGIONAL` )
<details> <summary>Hint</summary> You can use the `validation` block in the variable definition to restrict specific values for the `storage_class`. Use a regular expression or a condition to ensure the input is not in the restricted list.</details> 
<details> <summary>Solution</summary>

```bash
# variables.tf
variable "storage_class" {
  description = "The storage class of the bucket"
  type        = string

  validation {
    condition = !contains(["NEARLINE", "COLDLINE", "ARCHIVE", "MULTI_REGIONAL"], var.storage_class)
    error_message = "The storage class cannot be 'NEARLINE', 'COLDLINE', 'ARCHIVE', or 'MULTI_REGIONAL'."
  }
}

# main.tf
resource "google_storage_bucket" "bucket" {
  name         = "my-unique-bucket-name"
  location     = "US"
  storage_class = var.storage_class
}
```
</details>

---

## Task 5: 
- **Question:** Add some lables as below to the storage and pickup maintenance no from variable maintenance_no
  - teamname - sales
  - maintenance_no - 5436
<details> <summary>Hint</summary> You can use the `labels` argument in the `google_storage_bucket` resource to add key-value pairs for your custom labels. </details> <details> <summary>Solution</summary>

```bash
# variables.tf
variable "maintenance_no" {
  description = "The maintenance number for the storage bucket"
  type        = string
  default     = "5436"
}

# main.tf
resource "google_storage_bucket" "bucket" {
  name          = "my-unique-bucket-name"
  location      = "us-central1"

  labels = {
    teamname       = "sales"
    maintenance_no = var.maintenance_no
  }
}

```
</details>
</details>

---

## Task 6: 
- **Question:** now from the previous example validate the maintennace_no to be always digits and have 4 digits only.

<details> <summary>Hint</summary> Use the `validation` block in the variable definition to ensure the `maintenance_no` meets the criteria. Leverage a regular expression to check for exactly 4 digits. </details> <details> <summary>Solution</summary>

```bash
# variables.tf
# variables.tf
variable "maintenance_no" {
  description = "The maintenance number for the storage bucket"
  type        = string
  default     = "5436"

  validation {
    condition     = can(regex("^/d{4}$", var.maintenance_no))
    error_message = "The maintenance number must be exactly 4 digits (e.g., 1234)."
  }
}

# main.tf
resource "google_storage_bucket" "bucket" {
  name          = "my-unique-bucket"
  location      = "US"
  storage_class = "STANDARD"

  labels = {
    teamname       = "sales"
    maintenance_no = var.maintenance_no
  }
}
```
</details>
</details>

---

## Task 7: 
- **Question:** How can you ensure that a GCP bucket name follows the required format:  
     - Must be between 3 and 63 characters.
     - Can only contain lowercase letters, numbers, and hyphens.
     - Must start and end with a lowercase letter or number.

<details> <summary>Hint</summary> Use the `validation` block in the variable definition with a regex pattern to enforce bucket naming rules. </details> <details> <summary>Solution</summary>

```bash
# variables.tf
variable "bucket_name" {
  description = "The name of the GCP storage bucket"
  type        = string

  validation {
    condition = can(regex("^[a-z0-9]([-a-z0-9]{1,61}[a-z0-9])?$", var.bucket_name))
    error_message = <<EOT
    The bucket name must:
    - Be between 3 and 63 characters long.
    - Only contain lowercase letters, numbers, and hyphens.
    - Start and end with a lowercase letter or number.
    EOT
  }
}

# main.tf
resource "google_storage_bucket" "validated_bucket" {
  name          = var.bucket_name
  location      = "US"
  storage_class = "STANDARD"
}

```
</details>

