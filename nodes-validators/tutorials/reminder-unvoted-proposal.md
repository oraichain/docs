# **Tracking Unvoted Proposals for Oraichain Network Validators Using `cosmos-proposals-checker`**

## **Introduction**
As an **Oraichain validator**, it’s crucial to stay updated with governance proposals and ensure you **cast your votes**. Missing votes can affect your reputation and might even lead to delegation losses. The `cosmos-proposals-checker` tool simplifies the process of tracking unvoted proposals.

This tutorial walks you through:
1. **Downloading the Binary**
2. **Setting Up the Configuration**
3. **Creating a Systemd Service**

---

## **1. Download and Install `cosmos-proposals-checker`**
### **Step 1: Download the Latest Release**
Check the latest release from the [GitHub repository](https://github.com/QuokkaStake/cosmos-proposals-checker)

Example with version **v5.14.1**:
```sh
wget https://github.com/QuokkaStake/cosmos-proposals-checker/releases/download/v5.14.1/cosmos-proposals-checker_5.14.1_linux_amd64.tar.gz -O cosmos-proposals-checker.tar.gz
```

### **Step 2: Extract and Move the Binary to the System Apps Folder**
```sh
tar -xzvf cosmos-proposals-checker.tar.gz
chmod +x cosmos-proposals-checker
sudo mv cosmos-proposals-checker /usr/local/bin/
```

### **Step 3: Verify Installation**
Now you can run it from anywhere:
```sh
cosmos-proposals-checker --version
```
The output should be:
```
cosmos-proposals-checker version 5.14.1
```

---

## **2. Setting Up the Configuration**
Now, let's **download and configure the config file**.

### **Step 1: Download the Example Configuration**
For this tutorial, we will place the configuration file in `/root/config.yml`, but you can choose your own path.

```sh
wget https://raw.githubusercontent.com/QuokkaStake/cosmos-proposals-checker/5cb45561274c78d9e153919b3bb33e3b3f364231/config.example.toml -O /root/config.yml
```

### **Step 2: Edit the Configuration File**
Open the configuration file:
```sh
vi /root/config.yml
```

### **Step 3: Update the Chain Configuration**
Modify the file to match **Oraichain settings**. Below is an example:

```toml
[[chains]]
name = "Oraichain"
pretty-name = "Oraichain"
keplr-name = "Oraichain"
mintscan-prefix = ""
lcd-endpoints = ["https://lcd.orai.io", "https://oraichain-rest.publicnode.com"]
wallets = [
    { address = "orai1aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" }
]
```
Replace **`orai1aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`** with your **actual validator wallet address**.

### **Step 4: Set the Reporting Interval**
To schedule daily reports at **04:00 AM**, update:
```toml
interval = "0 4 * * *"
```

### **Step 5: Configure Notification Channels**
`cosmos-proposals-checker` supports:
- **Telegram**
- **PagerDuty**

Modify the notifier settings in the configuration file based on your preferred method.

---

## **3. Create a Systemd Service**
To **automate execution**, we will create a **systemd service**.

### **Step 1: Create the Service File**
```sh
sudo nano /etc/systemd/system/cosmos-proposals-checker.service
```

### **Step 2: Add the Following Configuration**
```ini
[Unit]
Description=Cosmos Proposals Checker
After=network.target

[Service]
ExecStart=/usr/local/bin/cosmos-proposals-checker --config /root/config.yml
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```
Make sure to replace `/root/config.yml` with your **actual configuration file path**.

---

### **Step 3: Reload and Start the Systemd Service**
```sh
sudo systemctl daemon-reload
sudo systemctl start cosmos-proposals-checker
sudo systemctl enable cosmos-proposals-checker
```

---

## **4. Check Logs and Service Status**
To verify that the service is running correctly:

### **Check the Service Status**
```sh
systemctl status cosmos-proposals-checker
```

### **View Logs in Real Time**
```sh
journalctl -u cosmos-proposals-checker -f
```

---

Now, your validator will **never miss governance votes again!** 🚀