# **Validator Image: Enhance Your Validator’s Identity**

A **validator image** is a visual representation of your validator, displayed on validator pages across different blockchain explorers.

### **Example Validator Pages:**
- **ORAI Scan:** [View Example](https://scan.orai.io/validators/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x)
- **Scanium:** [View Example](https://scanium.io/Oraichain/staking/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x)

---
## **How to Set or Update Your Validator Image**

### **On ORAI Scan (scan.orai.io)**
1. Navigate to your **validator page**.
2. **Double-click** on the validator image.
3. **Select** and upload your new image.
4. **Approve** the request to finalize the update.

---
### **On Scanium (scanium.io)**
Scanium fetches validator images directly from **Keybase**. Follow these steps to update your image:

#### **Step 1: Set Up a Keybase Profile**
- If you don’t have a **Keybase** account, create one.
- Retrieve your **public Keybase ID** (e.g., `2798DB6D73F30696`).

#### **Step 2: Update Your Validator Identity**
If your validator identity is not set, submit a transaction to link your Keybase ID:

```bash
oraid tx staking edit-validator \
--identity "your-keybase-id" \
--chain-id Oraichain \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.01orai \
-y
```

#### **Step 3: Change Your Keybase Avatar**
- Set your **Keybase profile picture** to the desired validator image.
- Scanium will automatically fetch and update the validator image from your Keybase account.

---
### **Final Thoughts**
Keeping your validator image updated enhances your validator’s visibility and credibility in the network. Follow the steps above to ensure your validator stands out!

🚀 **Make your validator recognizable today!**