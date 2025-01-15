# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with **Wireshark** and experiment with **Network Security Groups (NSGs)**.

---

## **Video Demonstration**

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com/results?search_query=azure+virtual+machines%2C+wireshark%2Cand+network+security+groups)

---

## **Environments and Technologies Used**

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

---

## **Operating Systems Used**

- Windows 10 (21H2)
- Ubuntu Server 20.04

---

## **High-Level Steps**

1. Create and Configure Azure Virtual Machines  
2. Configure Network Security Groups (NSGs)  
3. Capture and Analyze Network Traffic with Wireshark  
4. Experiment with Allow/Deny Rules in NSGs  

---

## **Detailed Steps and Observations**

### **Step 1: Create and Configure Azure Virtual Machines**

1. Log in to the **Azure Portal** at [portal.azure.com](https://portal.azure.com).
2. Create two VMs:
   - **Windows 10 VM** (used for RDP, HTTP, and general observations)
   - **Ubuntu Server VM** (used for SSH and ICMP traffic)
3. Ensure that both VMs are part of the **same virtual network (VNet)**.
4. Assign **public and private IP addresses** to each VM.

---

### **Step 2: Configure Network Security Groups (NSGs)**

1. Navigate to the **Azure Portal** and open the **Networking** tab for each VM.
2. Create inbound and outbound rules in the **NSG**:
   - **Allow SSH** (port 22) for the Ubuntu VM.
   - **Allow RDP** (port 3389) for the Windows 10 VM.
   - **Allow HTTP/HTTPS** (ports 80/443) for web traffic.
   - **Allow ICMP** for ping requests.
3. Test connectivity by:
   - Using `ping` to verify ICMP.
   - Connecting to the Ubuntu VM via SSH:
     ```bash
     ssh <username>@<public-IP-address>
     ```
   - Connecting to the Windows 10 VM via RDP.

---

### **Step 3: Capture and Analyze Network Traffic with Wireshark**

1. Download and install **Wireshark** on the Windows 10 VM.
2. Open **Wireshark** and select the **Ethernet interface**.
3. Start capturing network traffic and perform the following actions:
   - **RDP connection**: Observe packets related to port **3389**.
   - **HTTP request**: Open a web browser and navigate to `http://<server-ip>`. Observe **HTTP packets** on port **80**.
   - **ICMP request**: Ping the Ubuntu VM and observe the **ICMP echo request and reply**.
4. Stop the capture and filter packets by protocol (e.g., `tcp`, `udp`, `icmp`) for detailed inspection.

---

### **Step 4: Experiment with Allow/Deny Rules in NSGs**

1. In the **NSG settings** of the Azure portal, edit the **inbound rules**:
   - **Deny ICMP**: Create a rule to block ping requests.
   - **Deny HTTP/HTTPS**: Create a rule to block web traffic.
2. Test the following:
   - **Ping the Ubuntu VM**: Confirm that ping requests time out when ICMP is blocked.
   - **Open a web browser**: Attempt to access a website on the Ubuntu VM when HTTP/HTTPS traffic is blocked. Observe the connection failure.
3. Revert the **deny rules** to **allow** and retest connectivity to ensure that the NSG rules work as expected.

---

## **Actions and Observations**

- **ICMP Traffic:** Captured and confirmed successful ping requests and replies.
- **RDP Traffic:** Observed the use of **port 3389** during remote desktop sessions.
- **HTTP Traffic:** Captured GET and POST requests while browsing a web server.
- **NSG Rule Impact:** Verified that changing NSG rules immediately impacts traffic flow.

---

## **Additional Configuration Options**

- **Custom NSG Rules:**  
  Create custom rules to allow or deny specific IP ranges.
- **Azure Network Watcher:**  
  Use Azure's **Network Watcher** to troubleshoot and monitor VM connectivity.
- **Logging and Alerts:**  
  Enable NSG flow logs and configure alerts to monitor unusual network activity.

---

## **Final Notes and Best Practices**

- **Snapshots:**  
  Take snapshots of your VM states after configuring the NSGs and Wireshark to quickly restore in case of errors.
- **Security:**  
  Disable **public SSH and RDP** when not in use to avoid exposure to external threats.
- **Monitoring:**  
  Use **Azure Monitor** to track network usage and security-related events.
