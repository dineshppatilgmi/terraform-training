### **Sequence to Run Terraform Commands**

When using Terraform, it's essential to follow the correct sequence to ensure smooth execution and avoid errors. Here's the recommended order:

---

#### **Step 1: Initialize the Working Directory**
**Command**:

```Bash
terraform init
```

- Run this command first in your Terraform project directory.  
- It prepares your working directory by downloading provider plugins, configuring the backend, and initializing modules.  
- **Why?** Ensures that your environment is ready for Terraform operations.

---

#### **Step 2: Format the Terraform Files**
**Command**:  
```bash
terraform fmt
```

- Run this command to format your `.tf` files and maintain consistency in code style.  
- **Why?** Makes the configuration files clean, readable, and consistent.

---

#### **Step 3: Validate the Configuration**
**Command**:  
```bash
terraform validate
```

- This checks the syntax and logical correctness of your Terraform configuration files.  
- **Why?** Helps catch errors before planning or applying changes.

---

#### **Step 4: Generate an Execution Plan**
**Command**:  
```bash
terraform plan -out=tfplan
```

- Use this command to preview the changes Terraform will make to your infrastructure.  
- Save the plan file for later use by specifying the `-out` option.  
- **Why?** Provides visibility into the actions Terraform will perform, such as adding, changing, or destroying resources.

---

#### **Step 5: Apply the Changes**
**Command**:  
```bash
terraform apply tfplan
```

- Apply the changes outlined in the execution plan created in the previous step.  
- If no plan file is specified, Terraform will create and apply a new plan interactively.  
- **Why?** Deploys the desired state of your infrastructure as defined in the configuration files.


#### **Step 6: Delete the Infrastructure created using terraform**
**Command**:  
```bash
terraform destory
```

- The terraform destroy command is used to delete or destroy the resources that Terraform manages, effectively reversing any terraform apply operations.  
- **Why?** Completely removes all resources defined in your Terraform configuration from the infrastructure provider..

---

### **Example Workflow**
Here’s how it looks in practice:
```bash
# Step 1: Initialize Terraform
terraform init

# Step 2: Format files
terraform fmt

# Step 3: Validate the configuration
terraform validate

# Step 4: Create an execution plan
terraform plan -out=tfplan

# Step 5: Apply the changes
terraform apply tfplan

# Step 6: Delete the Infra
terraform destroy
```

This sequence ensures your infrastructure is managed efficiently, with minimal errors or surprises during deployment.