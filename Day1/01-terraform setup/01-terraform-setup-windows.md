### **Setting Terraform on Windows**

#### **1. Terraform Setup**

1. **Download Terraform**:  
   - Download Terraform from the [official Terraform downloads page](https://developer.hashicorp.com/terraform/install?product_intent=terraform).
2. **Extract the ZIP File**:  
   - Extract the Terraform executable (`terraform.exe`) to a desired directory, e.g., `C:\terraform`.
3. **Add Terraform to PATH**:
   - Open the **Start Menu**, search for "Environment Variables," and select **Edit the system environment variables**.
   - In the **System Properties** window, click **Environment Variables**.
   - In the **System variables** section, find the variable `Path` and click **Edit**.
   - Add the directory containing `terraform.exe` (e.g., `C:\terraform`) to the PATH.

4. **Verify Installation**:  
   Open a command prompt and run:  
   
```bash
   terraform --version
```

---

#### **2. Set GCP Provider Environment Variables**

Terraform's **GCP provider** requires credentials to interact with Google Cloud resources. These credentials can be set using the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

1. **Generate a Service Account Key**:
   - Go to the [Google Cloud Console](https://console.cloud.google.com).
   - Navigate to **IAM & Admin > Service Accounts >dinesh-patil**.
   - Create a new service account or use an existing one.
   - Grant the appropriate roles (e.g., `Editor`, `Owner`, or custom roles).
   - Download the key as a JSON file (e.g., `dinesh-patil.json`).

2. If you will inspect the json file it contents new line charactes in it terraform doesn't like that, so we need to remove it.
```Bash
# Run below command in powershell
(Get-Content dinesh-patil.json -Raw) -replace "`n", " " | Set-Content tf-dinesh-patil.json
```

3. **Set the Environment Variable**:
   - Place the JSON key file in a directory, e.g., `C:\gcp\tf-dinesh-patil.json`.
   - Open the **Environment Variables** settings as described earlier.
   - In the **System variables** section, click **New** and create a variable:
     - **Variable name**: `GOOGLE_APPLICATION_CREDENTIALS`
     - **Variable value**: Full path to the key file, e.g., `C:\gcp\tf-dinesh-patil.json`.

4. **Verify**:
   - Open a new command prompt and check the variable:
     ```bash
     echo %GOOGLE_APPLICATION_CREDENTIALS%
     ```
   - Ensure the output matches the JSON file path.

---

### for more details about authentication and setup please refer [this doc](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/getting_started)

#### **3. Set the `TF_LOG` Environment Variable for Debugging**
Terraform provides logging capabilities for debugging purposes using the `TF_LOG` environment variable.

1. **Set `TF_LOG`**:
   - Open the **Environment Variables** settings.
   - Add a new **System variable**:
     - **Variable name**: `TF_LOG`
     - **Variable value**: `DEBUG` (or other levels such as `INFO`, `ERROR`, etc.).

2. **Optional: Set Log Output File**:
   To log debug output to a file, add the `TF_LOG_PATH` variable:
   - **Variable name**: `TF_LOG_PATH`
   - **Variable value**: Path to the log file, e.g., `C:\terraform\terraform.log`.

3. **Verify**:
   - Open a new command prompt.
   - Run any Terraform command, e.g., `terraform plan`, and observe debug logs in the terminal or log file.

---

### **Example Verification**
1. **Terraform Setup**:
   ```bash
   terraform --version
   ```
   Verify that Terraform is available.

2. **GCP Provider**:
   ```bash
   echo %GOOGLE_APPLICATION_CREDENTIALS%
   ```
   Ensure the correct path is displayed.

3. **TF_LOG Debugging**:
   Run a Terraform command, e.g.:
   ```bash
   terraform plan
   ```
   If `TF_LOG` is set, debug information will appear in the terminal or log file (if `TF_LOG_PATH` is set).

---

These steps ensure that Terraform, GCP provider credentials, and logging are properly configured on a Windows system.