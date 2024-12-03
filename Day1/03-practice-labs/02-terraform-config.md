Refer [terraform documentation](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/storage_bucket) for the below exercise


### **1. Basic Bucket Creation**
**Task:** Write a Terraform configuration to create a GCP bucket with a unique name and default settings.  
- **Hint:** Use the `google_storage_bucket` resource with the `name` and `location` attributes.

---

### **2. Enable Bucket Versioning**
**Task:** Create a bucket with versioning enabled.  
- **Hint:** Use the `versioning` block within the `google_storage_bucket` resource.

---

### **3. Set a Retention Policy**
**Task:** Configure a bucket with a retention policy of 30 days.  
- **Hint:** Use the `retention_policy` block with the `retention_period` attribute.

---

### **4. Apply Labels to a Bucket**
**Task:** Create a bucket and apply labels like `environment` and `team`.  
- **Hint:** Use the `labels` attribute as a map.

---

### **5. Publicly Accessible Bucket**
**Task:** Configure a bucket that allows public read access.  
- **Hint:** Use the `google_storage_bucket_iam_binding` resource for granting `roles/storage.objectViewer` to `allUsers`.

---

### **6. Enforce Uniform Bucket Access**
**Task:** Create a bucket with uniform access control enabled.  
- **Hint:** Set the `uniform_bucket_level_access` attribute to `true`.

---

### **7. Lifecycle Rules**
**Task:** Write a configuration for a bucket with lifecycle rules to delete objects older than 90 days.  
- **Hint:** Use the `lifecycle_rule` block with `action` and `condition`.

---

### **8. Configure Logging for the Bucket**
**Task:** Enable access logs for a bucket and specify a destination bucket for log storage.  
- **Hint:** Use the `logging` block within the `google_storage_bucket` resource.

---

### **9. Add Storage Classes**
**Task:** Create a bucket with a storage class of `NEARLINE`.  
- **Hint:** Use the `storage_class` attribute.

---

### **10. Bucket Policy Only**
**Task:** Enforce a bucket policy where objects cannot be made public individually.  
- **Hint:** Use the `public_access_prevention` attribute set to `"enforced"`.

---

Complete each task by modifying the configuration to meet the requirements of the exercise. As you progress, try combining features like labels, versioning, and lifecycle rules in more complex configurations.

Please don't miss to destroy after completion of exercise