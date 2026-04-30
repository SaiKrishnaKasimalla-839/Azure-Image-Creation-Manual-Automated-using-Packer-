# Azure-Image-Creation-Manual-Automated-using-Packer-


# 🚀 Azure Image Creation (Manual & Automated using Packer)

## 📌 Objective

Create reusable VM images in Azure using:

* Manual method (Portal-based)
* Automated method using Packer

---

# 🖥️ Manual Image Creation

## 🔹 Steps

1. **Create Virtual Machine**

   * Select OS (Windows/Linux)
   * Configure size, networking, credentials

2. **Install Required Software**

   * Connect using RDP/SSH
   * Install tools (Java, Maven, Nginx, etc.)

3. **Stop VM**

   * Navigate to VM → Click **Stop**

4. **Capture Image**

   * Click **Capture**
   * Provide Image Name & Resource Group

5. **Create VM from Image**

   * Go to **Images**
   * Select created image → **Create VM**

6. **Verification**

   * Launch VM
   * Confirm installed software is present

---

## 🔁 Manual Flow

```text
VM Creation → Software Setup → Stop → Capture → Image → Reuse
```

---

# ⚡ Automated Image Creation using Packer

## 🔹 Setup

1. Download & extract Packer
2. Open folder in PowerShell
3. Verify installation:

   ```powershell
   packer version
   ```

---

## 🔹 Configuration File (`krishna.json`)

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "YOUR_CLIENT_ID",
    "client_secret": "YOUR_SECRET",
    "tenant_id": "YOUR_TENANT_ID",
    "subscription_id": "YOUR_SUBSCRIPTION_ID",

    "managed_image_resource_group_name": "ForImageRG",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "canonical",
    "image_offer": "0001-com-ubuntu-server-jammy",
    "image_sku": "22_04-lts",

    "location": "Japan East",
    "vm_size": "Standard_B1s"
  }],

  "provisioners": [{
    "type": "shell",
    "inline": [
      "sudo apt update -y",
      "sudo apt install -y default-jdk",
      "sudo apt install -y maven",
      "java -version",
      "mvn -version",
      "sudo waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ]
  }]
}
```

---

## 🔹 Execution

```powershell
.\packer.exe build .\krishna.json
```

---

## 🔹 Build Process

```text
Create Temporary VM
        ↓
Install Software (Java, Maven)
        ↓
Deprovision VM
        ↓
Capture Managed Image
        ↓
Delete Temporary Resources
```

---

## 🔹 Output

* Image Name: `myPackerImage`
* Resource Group: `ForImageRG`
* Location: Configured region

---

## 🔹 Create VM from Image

1. Azure Portal → **Images**
2. Select `myPackerImage`
3. Click **Create VM**
4. Launch instance

---

# ⚙️ Key Observations

* Build fails if any provisioning command fails
* VM size and region must support selected SKU
* Temporary resources are auto-deleted after build
* Image is created only after successful execution

---

# 📊 Manual vs Automated

| Aspect      | Manual | Packer |
| ----------- | ------ | ------ |
| Setup Time  | High   | Low    |
| Reusability | Medium | High   |
| Automation  | ❌      | ✅      |
| Consistency | ❌      | ✅      |

---

# 🎯 Outcome

* Successfully automated image creation
* Created reusable VM image with pre-installed tools
* Reduced manual configuration effort

---

# 👨‍💻 Author

Sai Krishna Kasimalla
