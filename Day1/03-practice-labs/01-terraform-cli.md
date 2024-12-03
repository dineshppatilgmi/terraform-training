# Terraform Command Exploration Guide


## **1. `terraform init`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Initialize a Terraform project.
- **Question**: What does the `-backend-config` attribute do in `terraform init`?
<details>
<summary>Hint</summary>
The `-backend-config` attribute allows you to pass backend configuration settings like storage bucket names or paths.
</details>
<details>
<summary>Solution</summary>

```bash
terraform init -backend-config="path=terraform-state.tfstate"
```
This sets the backend to use a specific state file path.
</details>


### Task 2: Disable plugin downloads during initialization.
- **Question**: How can you use the -get-plugins attribute in terraform init?
<details> <summary>Hint</summary> The `-get-plugins` flag controls whether or not provider plugins are downloaded. </details> <details> <summary>Solution</summary>
bash
Copy code
terraform init -get-plugins=false
This skips downloading plugins.

</details>
</details>

---

## **2. `terraform fmt`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Format all Terraform files in a directory.
- **Question**: How do you apply `terraform fmt` recursively?
<details>
<summary>Hint</summary>
The `-recursive` flag formats files in subdirectories.
</details>

<details>
<summary>Solution</summary>

```bash
terraform fmt -recursive
```
This formats all Terraform files in the directory and its subdirectories.
</details>

### Task 2: Check for formatting issues without fixing.
- **Question:** How do you use the -check attribute in terraform fmt?
<details> <summary>Hint</summary> The `-check` flag reports formatting issues without making changes. </details> 
<details> <summary>Solution</summary>
bash
Copy code
terraform fmt -check
This reports any files that are not properly formatted.  

</details>

### Task 3: Display only formatted files.
- **Question:** How can you use the -list attribute with terraform fmt?
<details> <summary>Hint</summary> The `-list` flag determines whether formatted files are displayed in the output. </details> <details> <summary>Solution</summary>
bash
Copy code
terraform fmt -list=true
This lists files that were reformatted.

</details>
</details>

---

## **3. `terraform validate`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Validate and output results in JSON.
- **Question**: How do you use the `-json` flag in `terraform validate`?
<details>
<summary>Hint</summary>
The `-json` flag outputs validation results in a machine-readable JSON format.
</details>

<details>
<summary>Solution</summary>

```bash
terraform validate -json
```
This validates the configuration and outputs the results in JSON format.
</details>

### Task 2: Validate a Terraform configuration.
- **Question:** How do you check a specific configuration directory?
<details> <summary>Hint</summary> You can specify the directory as an argument to the command. </details> <details> <summary>Solution</summary>

```bash
terraform validate ./example-dir
```

This validates the Terraform configuration in the example-dir directory.

</details>
</details>

---

## **4. `terraform plan`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Generate an execution plan with a specific output file.
- **Question**: How do you generate a plan with a specific output file?
<details>
<summary>Hint</summary>
The `-out` attribute specifies the output file for the plan.
</details>

<details>
<summary>Solution</summary>

```bash
terraform plan -out=planfile.tfplan
```
This saves the plan to `planfile.tfplan`.
</details>

### Task 2: Show only specific resources in the plan.
- **Question:** How do you use -target to focus on specific resources?
<details> <summary>Hint</summary> The `-target` flag limits the scope of the plan to specific resources. </details> <details> <summary>Solution</summary>

```bash

terraform plan -target=google_storage_bucket.trainingstorage
```

This shows the plan for only the trainingstorage resource.

</details>

</details>

---

## **5. `terraform apply`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Apply changes automatically with confirmation bypass.
- **Question**: How can you skip the confirmation prompt and specify a state file?
<details>
<summary>Hint</summary>
Use the `-auto-approve` and `-state` flags to automate the process.
</details>

<details>
<summary>Solution</summary>

```bash
terraform apply -auto-approve -state=custom.tfstate
```
This applies the plan without confirmation and saves the state to `custom.tfstate`.
</details>

</details>

---

## **6. `terraform destroy`**
<details>
<summary>Explore tasks and solutions</summary>

### Task 1: Destroy resources without confirmation.
- **Question**: How do you bypass the confirmation prompt?
<details>
<summary>Hint</summary>
Use the `-auto-approve` flag.
</details>

<details>
<summary>Solution</summary>

```bash
terraform destroy -auto-approve
```
This destroys resources without asking for confirmation.
</details>

</details>

---