### **Setting Up Terraform on Linux**

#### **1. Terraform Setup**

1. **Download Terraform**:  
   - Download Terraform from the [official Terraform downloads page](https://developer.hashicorp.com/terraform/install?product_intent=terraform) or use a package manager like `wget` or `curl`:
     ```bash
     wget https://releases.hashicorp.com/terraform/<version>/terraform_<version>_linux_amd64.zip
     ```
     Replace `<version>` with the desired Terraform version (e.g., `1.5.7`).

2. **Install Terraform**:  
   - Unzip the downloaded file:
     ```bash
     unzip terraform_<version>_linux_amd64.zip
     ```
   - Move the executable to `/usr/local/bin`:
     ```bash
     sudo mv terraform /usr/local/bin/
     ```
   - Verify the installation:
     ```bash
     terraform --version
     ```
3. **use native package manager to install terraform**  
   
    ```bash
    wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    sudo apt update && sudo apt install terraform
    ```
    
---

#### **2. Set GCP Provider Environment Variables**

Terraform’s **GCP provider** requires the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to locate the JSON credentials file.

1. **Generate a Service Account Key**:
   - Go to the [Google Cloud Console](https://console.cloud.google.com).
   - Navigate to **IAM & Admin > Service Accounts**.
   - Create a new service account or use an existing one.
   - Grant the appropriate roles (e.g., `Editor`, `Owner`, or custom roles).
   - Download the key as a JSON file (e.g., `dinesh-patil.json`).

2. If you will inspect the json file it contents new line charactes in it terraform doesn't like that, so we need to remove it.
```Bash
# Run below command in bash
cat dinesh-patil.json | tr -s '\n' ' ' > tf-dinesh-patil.json
```


3. **Move the Key to a Secure Location**:
   - Place the key file in a directory, e.g., `/home/username/.gcp/`.
     ```bash
     mkdir -p ~/.gcp
     mv tf-dinesh-patil.json ~/.gcp/
     ```

3. **Set the `GOOGLE_APPLICATION_CREDENTIALS` Environment Variable**:
   - Add the following line to your shell configuration file (`~/.bashrc`, `~/.zshrc`, or equivalent):
     ```bash
     export GOOGLE_APPLICATION_CREDENTIALS="$HOME/.gcp/tf-dinesh-patil.json"
     ```
   - Apply the changes:
     ```bash
     source ~/.bashrc
     ```

4. **Verify**:
   - Check if the variable is set correctly:
     ```bash
     echo $GOOGLE_APPLICATION_CREDENTIALS
     ```
   - The output should display the path to the key file.

---

#### **3. Set the `TF_LOG` Environment Variable for Debugging**

Terraform’s logging capabilities can help debug issues. The `TF_LOG` variable controls the logging level.

1. **Set the `TF_LOG` Variable**:
   - Add this line to your shell configuration file:
     ```bash
     export TF_LOG=DEBUG
     ```
   - Apply the changes:
     ```bash
     source ~/.bashrc
     ```

2. **Optional: Set Log Output to a File**:
   - Add this line to the shell configuration file:
     ```bash
     export TF_LOG_PATH="$HOME/terraform-debug.log"
     ```
   - Apply the changes:
     ```bash
     source ~/.bashrc
     ```

3. **Verify**:
   - Run a Terraform command to test logging, e.g.:
     ```bash
     terraform plan
     ```
   - If `TF_LOG_PATH` is set, debug logs will be written to `~/terraform-debug.log`.

---

### **Example Verification**
1. **Terraform Setup**:
   ```bash
   terraform --version
   ```

2. **GCP Provider**:
   ```bash
   echo $GOOGLE_APPLICATION_CREDENTIALS
   ```
   Ensure it points to the correct JSON file path.

3. **TF_LOG Debugging**:
   Run a Terraform command, e.g.:
   ```bash
   terraform plan
   ```
   Debug information should appear in the terminal or in the specified log file.

---

### **Example Snippet for `~/.bashrc` or `~/.zshrc`**
```bash
# Terraform setup
export PATH=$PATH:/usr/local/bin

# GCP credentials
export GOOGLE_APPLICATION_CREDENTIALS="$HOME/.gcp/gcp-key.json"

# Terraform logging
export TF_LOG=DEBUG
export TF_LOG_PATH="$HOME/terraform-debug.log"
```

---

With these steps, you’ll have a fully configured environment for Terraform on Linux, with support for the GCP provider and logging for debugging.