# Configure AAA Authentication on Cisco Routers

## Part 1: Configure Local AAA Authentication for Console Access on R1

### Step 1: Test Connectivity
On PC-A, open the Command Prompt and ping other PCs:
```bash
ping 192.168.2.3  # Ping PC-B
ping 192.168.3.3  # Ping PC-C
```
On PC-B:
```bash
ping 192.168.3.3  # Ping PC-C
```

### Step 2: Configure a Local Username on R1
On R1, enter global configuration mode and create the user account:
```bash
R1> enable
R1# configure terminal
R1(config)# username Admin1 secret admin1pa55
R1(config)# exit
```

### Step 3: Enable AAA and Configure Local Authentication for Console
```bash
R1# configure terminal
R1(config)# aaa new-model
R1(config)# aaa authentication login default local
R1(config)# exit
```

### Step 4: Apply AAA Authentication to Console Line
```bash
R1# configure terminal
R1(config)# line console 0
R1(config-line)# login authentication default
R1(config-line)# exit
```

### Step 5: Verify Console Login Authentication
Exit to test the login:
```bash
R1# exit
```
When prompted, enter:
```
Username: Admin1
Password: admin1pa55
```

## Part 2: Configure Local AAA Authentication for VTY Lines on R1

### Step 1: Configure Domain Name and RSA Key
```bash
R1# configure terminal
R1(config)# ip domain-name ccnasecurity.com
R1(config)# crypto key generate rsa
  How many bits in the modulus [512]: 1024
```

### Step 2: Configure AAA Authentication for SSH Logins
```bash
R1(config)# aaa authentication login SSH-LOGIN local
```

### Step 3: Apply AAA to VTY Lines and Allow Only SSH
```bash
R1(config)# line vty 0 4
R1(config-line)# login authentication SSH-LOGIN
R1(config-line)# transport input ssh
R1(config-line)# exit
```

### Step 4: Verify SSH Login
On PC-A, open the command prompt and SSH into R1:
```bash
ssh -l Admin1 192.168.1.1
```
Enter `admin1pa55` when prompted.

## Part 3: Configure Server-Based AAA Authentication Using TACACS+ on R2

### Step 1: Configure a Backup Local Username
```bash
R2# configure terminal
R2(config)# username Admin2 secret admin2pa55
```

### Step 2: Verify TACACS+ Server Configuration
On R2, check if the TACACS+ server has an entry:
```bash
R2# show running-config | include tacacs
```

### Step 3: Configure TACACS+ Server
```bash
R2(config)# tacacs-server host 192.168.2.2
R2(config)# tacacs-server key tacacspa55
```

### Step 4: Configure AAA Authentication Using TACACS+
```bash
R2(config)# aaa new-model
R2(config)# aaa authentication login default group tacacs+ local
```

### Step 5: Apply AAA Authentication to Console
```bash
R2(config)# line console 0
R2(config-line)# login authentication default
R2(config-line)# exit
```

### Step 6: Verify Authentication
Exit and try logging in:
```bash
R2# exit
```
Login using:
```
Username: Admin2
Password: admin2pa55
```

## Part 4: Configure Server-Based AAA Authentication Using RADIUS on R3

### Step 1: Configure a Backup Local Username
```bash
R3# configure terminal
R3(config)# username Admin3 secret admin3pa55
```

### Step 2: Verify RADIUS Server Configuration
On R3, check if the RADIUS server has an entry:
```bash
R3# show running-config | include radius
```

### Step 3: Configure RADIUS Server
```bash
R3(config)# radius-server host 192.168.3.2
R3(config)# radius-server key radiuspa55
```

### Step 4: Configure AAA Authentication Using RADIUS
```bash
R3(config)# aaa new-model
R3(config)# aaa authentication login default group radius local
```

### Step 5: Apply AAA Authentication to Console
```bash
R3(config)# line console 0
R3(config-line)# login authentication default
R3(config-line)# exit
```

### Step 6: Verify Authentication
Exit and try logging in:
```bash
R3# exit
```
Login using:
```
Username: Admin3
Password: admin3pa55
```

### Step 7: Check Results
Click **Check Results** in Packet Tracer and verify completion. It should show **100% completion**.

✅ You have successfully configured AAA authentication using local, TACACS+, and RADIUS authentication on R1, R2, and R3. 🚀

