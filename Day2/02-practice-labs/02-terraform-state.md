# Terraform state and workspaces Tasks


## Task 1: 
- **Question**: create two workspaces dev and sbx. Use two different tfvars file for dev and sbx. use variable `bucket_name_suffix` to get bucket name. How do you ensure that the bucket name is prefixed with sbx- or dev- depending on the workspace?.
<details>
<summary>Hint</summary>
Use the `${terraform.workspace}` variable to dynamically set prefixes for bucket names.
</details>
<details>
<summary>Solution</summary>

```bash
# variables.tf
variable "bucket_name_suffix" {
  description = "The suffix for the bucket name"
  type        = string
}

# main.tf
resource "google_storage_bucket" "workspace_bucket" {
  name          = "${terraform.workspace}-${var.bucket_name_suffix}"
  location      = "US"
  storage_class = "STANDARD"
}

# Use two different tfvars file for dev and sbx
# For sbx workspace: `terraform workspace select sbx` then use bucket_name_suffix="my-app"
# Resulting bucket name: "sbx-my-app"

```

</details>

## Task 2: 
- **Question**: How can you ensure the storage bucket is created in us-central1 for sbx and us-central2 for dev workspaces?.
<details>
<summary>Hint</summary>
Define a variable of type `map` where keys are workspace names, and values are bucket locations.
</details>
<details>
<summary>Solution</summary>

```bash
# variables.tf

variable "bucket_name_suffix" {
  description = "The suffix for the bucket name"
  type        = string
}

variable "workspace_location_map" {
  description = "Mapping of workspaces to their respective bucket locations"
  type        = map(string)
  default = {
    sbx = "us-central1"
    dev = "us-central2"
  }
}

# main.tf
resource "google_storage_bucket" "workspace_bucket" {
  name          = "${terraform.workspace}-${var.bucket_name_suffix}"
  location      = var.workspace_location_map[terraform.workspace]
  storage_class = "STANDARD"
}


```

</details>

## Task 3: 
- **Question**: How do you list all the resources managed by Terraform in a specific workspace like sbx or dev?.
<details>
<summary>Hint</summary>
Use the `terraform state list` command after switching to the appropriate workspace. 
</details>
<details>
<summary>Solution</summary>

```bash
# Switch to sbx workspace
terraform workspace select sbx

# List all resources in sbx workspace
terraform state list

# Switch to dev workspace
terraform workspace select dev

# List all resources in dev workspace
terraform state list

```

</details>

## Task 4: 
- **Question**: How do you remove a specific resource from the state file in the sbx workspace without deleting the actual resource?.
<details>
<summary>Hint</summary>
Use the `terraform state rm` command carefully to remove resources from the state file.  
</details>
<details>
<summary>Solution</summary>

```bash
# Switch to sbx workspace
terraform workspace select sbx

# Remove a specific resource (e.g., google_storage_bucket.workspace_bucket)
terraform state rm google_storage_bucket.workspace_bucket


```

</details>
