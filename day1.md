# Day 1: Networking in RHEL 9.3 (Linux Training)

---

## Lab

### **Guide for Setting up RHEL 9.3 Server on Oracle VirtualBox**

---

### 1. Network Setup

- **Host-Only Network**: Used for private networking between virtual machines on the host system.
- **NAT Network**: Provides network address translation for connecting the virtual machine to the internet, similar to AWS Internet Gateway (IGW).

---

### 2. Create a New VM

- **Steps**:
    1. **Navigate**: Go to **Machine Menu > New**.
    2. **Name**: Enter the desired VM name (e.g., `linuxServer`).
    3. **ISO Selection**: Browse to `labuser > Desktop > allISO > rhel9.3`.
    4. **Unattended Installation**: Check the box **Skip unattended allocation**.
    5. **Hardware Configuration**:
        - **RAM**: 2 GB (modifiable based on system capacity).
        - **CPU**: 1 Core (can be increased for higher performance).
        - **Hard Disk**: 20 GB (modifiable based on requirements).
    6. Click **Finish** to create the VM.

---

### 3. VM Settings

- **System Settings**:
    - **Boot Order**: Arrange as follows:
        1. **Optical**
        2. **Hard Disk**
        - **Note**: Uncheck other boot options.
    - **Pointing Device**: Set to **USB Tablet**.
- **Storage**: Ensure the ISO file is mounted as a DVD.
- **Network**:
    - **Adapter 1**: Set as **Host-Only Adapter**.

---

### 4. Start the VM

- Click the **Start (Green) Button**.
- On the boot screen, select **RHEL 9.3** using the arrow keys and press **Enter**.

---

### 5. Installation Process

- **Language**: Select **English (India)** and click **Continue**.
- **Software Selection**: Choose **Server with GUI**.
- **Installation Destination**: Select and click **Done** to clear the red notification.

---

### 6. Configure Users

- **Root Account**:
    - Uncheck **Lock Root Account**.
    - Set Root Password (example): `wipro123` (confirm by typing twice).
- **Normal User Creation**:
    - **Username**: `user1` (modifiable).
    - **Password**: `12345` (modifiable).
- Click **Begin Installation**.

---

### 7. Finalize Installation

- **File System Creation**: Wait for the process to complete.
- **Package Download & Installation**: Let all necessary packages download and install automatically.

---

### Post-Installation Steps

1. **Reboot System**.
2. **Login**: Use the root credentials set during installation.
3. **Set Additional Credentials if Required**.

---

### **Network Configuration Using nmcli**

---

### 1. Create a New Network Connection

- **Command**:
    
    ```bash
    nmcli connection add type ethernet con-name himanshu-net ifname enp0s3 ipv4.method manual ipv4.addresses 192.168.10.10/24 ipv4.gateway 192.168.10.1 ipv4.dns "198.168.10.201 198.168.10.202" connection.autoconnect yes
    
    ```
    
    - **Fixed Parameters**: `nmcli connection add type ethernet`, `ipv4.method`.
    - **Configurable Parameters**:
        - `con-name himanshu-net`: Changeable connection name.
        - `ifname enp0s3`: Adjust based on interface name.
        - `ipv4.addresses`, `ipv4.gateway`, and `ipv4.dns`: Modifiable IP values.

### 2. Checking Device Status

- **Command**:
    
    ```bash
    nmcli device status | grep ethernet
    
    ```
    
    - Displays the status of Ethernet devices.

---

### **Modify Network Configuration**

- **File Location**: `/etc/NetworkManager/systemconnections/`
- **Editing Configuration**:
    1. **Navigate**:
        
        ```bash
        cd /etc/NetworkManager/systemconnections/
        
        ```
        
    2. **List Files**:
        
        ```bash
        ls
        
        ```
        
    3. **Edit Configuration**:
        
        ```bash
        vi <connection-name>.nmconnection
        
        ```
        
    - Save changes with `:x!`.

---

### **Apply Changes**

- **Reload Connection**:
    
    ```bash
    nmcli connection reload himanshu-net
    
    ```
    
- **Restart NetworkManager**:
    
    ```bash
    systemctl restart NetworkManager
    
    ```
    
- **Bring Connection Up**:
    
    ```bash
    nmcli connection up himanshu-net
    
    ```
    

---

### **Automatic IP Address Allocation**

1. **Delete Existing Connection**:
    
    ```bash
    nmcli connection delete himanshu-net
    
    ```
    
2. **Machine Settings**:
    - **Navigate**: `Machine Menu > Settings > Network > Attach to Host-Only - NAT Network`.
3. **Add Connection with Auto IP**:
    
    ```bash
    nmcli connection add type ethernet con-name himanshu-net ifname enp0s3 ipv4.method auto
    
    ```
    
4. **Check Connection Status**:
    
    ```bash
    nmcli device status | grep ethernet
    
    ```
    

---

### **Key Network Concepts**

- **IP Address Classes**:
    - **Class-A**: 10.0.0.0 - 10.255.255.255 (Reserved for Private)
    - **Class-B**: 172.16.0.0 - 172.31.255.255 (Reserved for Private)
    - **Class-C**: 192.168.0.0 - 192.168.255.255 (Reserved for Private)
- **/24 Subnet**: `11111111.11111111.11111111.00000000` (255.255.255.0).
- **Default Gateway**: Acts as the point of entry/exit for networks.
- **DNS Server**: Handles name resolution (name ↔ IP).

---

This structured guide aligns well for your Notion workspace, offering clear explanations, toggleable sections, and well-marked configurable vs. fixed parameters.
