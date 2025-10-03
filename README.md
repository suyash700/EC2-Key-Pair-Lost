# 🔑 EC2 Key Pair Recovery using Volume Attachment

This guide explains how to recover SSH access to an old EC2 instance if the original key pair (`old-key-pair.pem`) is lost, by attaching its root volume to a new instance and injecting a new key.

---

## 🖥️ Instance & Volume Details

- **Old Instance**  
  - Name: `old-instance`  
  - Volume: `old-volume (8GB)`  
  - Key Pair: `old-key-pair.pem` (❌ lost)
 
    <img width="1919" height="979" alt="Screenshot 2025-10-03 190317" src="https://github.com/user-attachments/assets/ffa7af75-e5f9-47e1-8968-8a0eb107b1df" />

    <img width="1919" height="1079" alt="Screenshot 2025-10-03 190237" src="https://github.com/user-attachments/assets/7cf9f06f-5315-43af-a583-6158eb95b3d1" />


- **New Instance**  
  - Name: `new-instance`  
  - Volume: `new-volume (15GB)`  
  - Key Pair: `new-key-pair.pem` (✅ working)  

---

## 🚀 Recovery Process

### 1️⃣ SSH into Instances
- If the old key still exists:

ssh -i old-key-pair.pem ubuntu@<old-instance-public-ip>
<img width="1313" height="270" alt="Screenshot 2025-10-03 190932" src="https://github.com/user-attachments/assets/3d1ef665-b264-43b5-beae-b202d6452537" />



## 2️⃣ Detach Old Volume

In AWS Console → EC2 → Volumes

Select old-volume (8GB) and Detach it from old-instance.

<img width="1919" height="976" alt="Screenshot 2025-10-03 191219" src="https://github.com/user-attachments/assets/b1e7b3f0-9919-49ec-a0a1-c82e548727b4" />

## DeAttach it from old-instance(1st Instance)

<img width="1919" height="975" alt="Screenshot 2025-10-03 191242" src="https://github.com/user-attachments/assets/01f0b9eb-d8b6-4b01-a024-e0cc4b493e21" />


## Attach it to new-instance(2nd Instance)
3️⃣ Attach Old Volume to New Instance

Attach old-volume to new-instance as a secondary volume.
It will usually appear as /dev/nvme1n1.

<img width="1919" height="977" alt="Screenshot 2025-10-03 191340" src="https://github.com/user-attachments/assets/b80473d0-49c6-4617-9849-bb2f327c62d9" />


## 4️⃣ Mount Old Volume on New Instance
  lsblk -fp        # identify old volume (check /dev/nvme1n1p1)
  sudo mkdir /mnt/oldroot
  sudo mount /dev/nvme1n1p1 /mnt/oldroot

  
<img width="1675" height="345" alt="Screenshot 2025-10-03 200819" src="https://github.com/user-attachments/assets/602a6171-9d6b-4ba8-a990-b0720cead7a5" />

## 5️⃣ Add New Key to Authorized Keys
Navigate into the old root volume’s .ssh directory:

<img width="1624" height="360" alt="Screenshot 2025-10-03 200827" src="https://github.com/user-attachments/assets/29bd064a-bddb-49f6-96e0-aba4d18d1dcf" />

## 6️⃣ Unmount & Detach Old Volume

<img width="1109" height="182" alt="Screenshot 2025-10-03 200840" src="https://github.com/user-attachments/assets/4a5200bb-8707-4666-ac28-e62d3411fba9" />

## 7️⃣ Reattach Old Volume to Old Instance

Attach old-volume back to old-instance as its root volume (/dev/sda1).
<img width="1919" height="973" alt="Screenshot 2025-10-03 194250" src="https://github.com/user-attachments/assets/d3ad5030-8872-4eae-8906-81e63e59de15" />

## 8️⃣ SSH into Old Instance with New Key
ssh -i new-key-pair.pem ubuntu@<old-instance-public-ip>

🎉 You have successfully restored SSH access to the old instance!
<img width="1919" height="1079" alt="Screenshot 2025-10-03 194658" src="https://github.com/user-attachments/assets/608be539-ae68-4801-828b-f11a618d0650" />

## QUICK CHECK for VOLUMES


<img width="1919" height="978" alt="Screenshot 2025-10-03 195006" src="https://github.com/user-attachments/assets/551c482b-1c93-4aad-b0ee-f93a6f763e1c" />

<img width="1919" height="977" alt="Screenshot 2025-10-03 195024" src="https://github.com/user-attachments/assets/2b3f1245-5d2e-4587-a254-40868aacb933" />

## Author : Suyash Dahitule


