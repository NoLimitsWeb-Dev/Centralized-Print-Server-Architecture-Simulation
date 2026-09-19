# Enterprise-Centralized-Print-Server-Architecture-Simulation
A simulated enterprise Local Area Network (LAN) featuring a centralized Print Server infrastructure and HTTP management dashboard built in Cisco Packet Tracer to demonstrate resource isolation, static IP budgeting, and Layer 2/3 traffic optimization.
## 🎯 Project Objective

In enterprise networks, mapping client machines directly to individual network printer IPs causes massive administrative overhead and scaling issues. This project demonstrates a centralized network print server model where system administrators and end-users monitor physical print hardware status dynamically through an integrated HTTP management dashboard hosted on a local static server.

To overcome platform simulation limitations, a custom **HTTP management dashboard** was built and hosted natively on the server node. This allows network administrators and end-users to dynamically audit network printer health, IP allocations, and queue states through an internal web interface.
---

### 🌐 Network Topology Components
```
               [ 192.168.1.0 /24 Enterprise LAN ]
               
                      +-------------------+

                      |   Print-Server    | (Centralized Dashboard)
                      |   192.168.1.10    |
                      +---------+---------+
                                |
                                | (FastEthernet 0/1)
                                |
                      +---------+---------+

                      |                   |
                      |   Cisco Catalyst  |
                      |    2960 Switch    |
                      |                   |
                      +----+---+-----+----+

                           |   |     |    
       +-------------------+   |     +-------------------+

       | (Fa 0/2)              | (Fa 0/3)                | (Fa 0/4)
       |                       |                         |
+------+------+         +------+------+           +------+------+

|  PC-Admin   |         |  PC-Sales   |           |  Printer-A  | (HR_LaserJet_A)
| 192.168.1.51|         | 192.168.1.52|           | 192.168.1.21|
+-------------+         +-------------+           +-------------+
                                                         | (Fa 0/5)
                                                         |
                                                  +------+------+

                                                  |  Printer-B  | (Sales_Color_B)
                                                  | 192.168.1.22|
                                                  +-------------+
```

To make the simulation realistic, we will build a standard small office network. Drag and drop the following components into your Packet Tracer workspace:
* 1 Server: Rename this to Print-Server.
* 2 Network Printers: Rename them to Printer-Office-A and Printer-Office-B.
* 2 End Devices: Add two PCs or Laptops (e.g., PC-Admin and PC-Sales).
* 1 Switch: Use a standard 2960 Switch to connect all devices.
* Connections: Use Copper Straight-Through cables to connect every device to the switch.
<img width="1141" height="1012" alt="image" src="https://github.com/user-attachments/assets/ada18ae6-bf88-4476-b5c2-38f04f3c7f07" />
---

### 🔢 IP Addressing Plan
For a professional portfolio project, it is best practice to use a structured IP plan. We will use static IPs for the infrastructure (Server and Printers) and DHCP or static for the PCs.

| Device Name          |          | IP Address           |             | Subnet Mask          |            | Default Gateway |         
---
| Print-Server         |          | 192.168.1.10         |             | 255.255.255.0        |            | 192.168.1.1     |
---
| Printer-Office-A     |          | 192.168.1.21         |             | 255.255.255.0        |            | 192.168.1.1     | 
---
| Printer-Office-B     |          | 192.168.1.22         |             | 255.255.255.0        |            | 192.168.1.1     | 
---
| PC-Admin             |          | 192.168.1.51         |             | 255.255.255.0        |            | 192.168.1.1     |
---
| PC-Sales             |          | 192.168.1.52         |             | 255.255.255.0        |            | 192.168.1.1     |
---

### ⚙️ Step-by-Step Configuration

Step 1: Configure the Network Printers
1. Click on Printer-Office-A and go to the Config tab.
<img width="1141" height="956" alt="image" src="https://github.com/user-attachments/assets/8de6e343-528b-4941-8974-7b554f6b7876" />

2. Under Global -> Settings, set the Gateway to 192.168.1.1.
<img width="1130" height="330" alt="image" src="https://github.com/user-attachments/assets/7cf4d096-5f3b-410d-ac02-7d6c0207e47c" />

3. Under Interface -> FastEthernet0, set the IP assignment to Static and enter 192.168.1.21 with a Subnet Mask of 255.255.255.0.
<img width="1142" height="528" alt="image" src="https://github.com/user-attachments/assets/f616cd2f-7a41-4900-9b96-1e99791581dd" />

4. Repeat this process for Printer-Office-B using its respective IP (192.168.1.22)
---

Step 2: Configure the Client PCs

1. Click on PC-Admin, go to Desktop -> IP Configuration, and assign 192.168.1.51.
<img width="1132" height="963" alt="image" src="https://github.com/user-attachments/assets/9ba91d00-bdf1-45c3-8e59-1c6ea88541cf" />

2. Go to Desktop -> Command Prompt and type ping 192.168.1.10 to ensure the PC can talk to the Print Server.
<img width="1138" height="325" alt="image" src="https://github.com/user-attachments/assets/b9b4a770-1fe6-4841-a2d5-bc98afcfa722" />

3. Repeat IP configuration for PC-Sales using 192.168.1.52.


Step 3: Configure the Print Server
1. Click on Print-Server and go to the Desktop tab -> IP Configuration.
<img width="1141" height="640" alt="image" src="https://github.com/user-attachments/assets/b5cb800a-f72c-424a-ab7d-ab2fcd0c3af1" />

2. Set the static IP to 192.168.1.10 and Gateway to 192.168.1.1.
3. Go to the Services tab and click on PRINT.
<img width="1137" height="477" alt="image" src="https://github.com/user-attachments/assets/00b08d7b-1d54-46fa-bfac-b5a313b0061c" />

---
Configuration Glitch: 
---

In standard versions of Cisco Packet Tracer, there is actually no dedicated "PRINT" service tab under the Server device's Services menu. Packet Tracer focuses primarily on core networking protocols (like HTTP, DHCP, DNS, and FTP) rather than local OS peripheral sharing like a Windows Print Server.
Therefore to simulate a print server for my portfolio without that tab, I can use one of two highly professional workarounds options:

* Option 1: The HTTP/Web Portal Workaround
  In the real world, enterprise print servers (like PaperCut or Xerox Workplace) are managed via a web interface. I can simulate this beautifully using the HTTP Service on my server.
* Option 2: The Direct Network Printer Simulation
  I use this if I want to simulate sending an actual print job packet across the network switch:

  But option 2 as its limitation - Cisco Packet Tracer's internal Text Editor does not have a "Print" button. Because Packet Tracer is purely a software and network traffic simulator, it doesn't replicate localized operating system tasks like sending documents to a spooler. Which makes option 1 preferred.
<img width="1127" height="490" alt="image" src="https://github.com/user-attachments/assets/bb0af2a6-ee44-49aa-9e00-9e48a1d2aabb" />

```
Therefore using the HTTP Web Portal workaround transforms this from a basic connectivity lab into an enterprise-level systems integration project.
```

### 🛠️ Step-by-Step Configuration

Option 1: The HTTP/Web Portal Workaround

Step 1:
1. Click on your Print-Server -> Services tab -> HTTP.
2. Ensure HTTP and HTTPS are both set to On.
3. Click edit next to the index.html file.
<img width="1150" height="442" alt="image" src="https://github.com/user-attachments/assets/f38d631e-abe0-4f23-af16-e402ba2468bc" />

4. Replace the HTML code with a clean, simple print management dashboard. Paste this code:
<img width="1131" height="977" alt="image" src="https://github.com/user-attachments/assets/79d1de3d-2e7d-4eed-b3d7-4566cb7c9a4d" />

```
<!DOCTYPE html>
<html>
<head>
    <title>Enterprise Print Management Portal</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 30px; background-color: #f4f6f9; }
        .container { max-width: 700px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        h2 { color: #2c3e50; border-bottom: 2px solid #34495e; padding-bottom: 10px; text-align: center; }
        .meta { font-size: 14px; color: #555; text-align: center; margin-bottom: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #34495e; color: white; }
        tr:nth-child(even) { background-color: #f9f9f9; }
        .status-ready { color: #27ae60; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <h2>🖨️ Centralized Print Server Management Dashboard</h2>
        <div class="meta">
            <strong>Server Hostname:</strong> Print-Server | 
            <strong>IP Address:</strong> 192.168.1.10 | 
            <strong>System Status:</strong> <span class="status-ready">ONLINE</span>
        </div>
        <table>
            <tr>
                <th>Printer Name</th>
                <th>Network IP</th>
                <th>Status</th>
                <th>Active Queues</th>
            </tr>
            <tr>
                <td>HR_LaserJet_A</td>
                <td>192.168.1.21</td>
                <td><span class="status-ready">Ready</span></td>
                <td>0 Jobs Pending</td>
            </tr>
            <tr>
                <td>Sales_Color_B</td>
                <td>192.168.1.22</td>
                <td><span class="status-ready">Ready</span></td>
                <td>0 Jobs Pending</td>
            </tr>
        </table>
    </div>
</body>
</html>
```

5. How to test/demonstrate it: Go to PC-Admin -> Desktop -> Web Browser. Type 192.168.1.10 in the URL bar. This proves the end-users can reach and interact with the centralized print server infrastructure.
<img width="1127" height="896" alt="image" src="https://github.com/user-attachments/assets/0b8476f1-9ea2-44e8-ab99-f8da875ea207" />

### ✅ Further Verification: 

Run a Live Connectivity Test (Ping Verification)

1. To see the actual network handshake happen in real time:
* Click on PC-Admin ➡️ Desktop tab ➡️ Command Prompt.
* Type ping 192.168.1.10 and press Enter.
* You will instantly see four successful replies: Reply from 192.168.1.10: bytes=32 time<1ms TTL=128.3.
<img width="1141" height="450" alt="image" src="https://github.com/user-attachments/assets/c2488b7f-6fa7-4947-8fe7-023a58b40002" />

---
2. Check the Physical Link Lights
Look closely at the lines (cables) connecting your devices to the 2960 Switch:
* You should see green dots on both ends of every cable.
* Green dots indicate that the physical layer is up, and STP (Spanning Tree Protocol) has finished converging, meaning the switch ports are actively forwarding traffic in real time.
<img width="1341" height="743" alt="image" src="https://github.com/user-attachments/assets/2e14e35d-2cb0-4593-9ad9-e73f2d494a1e" />



