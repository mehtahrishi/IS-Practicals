# ping from pc-a to pc-c 
ping 192.168.3.3


R1(config) # license boot module c1900 technology - packaage securityk9

accept 
write memory 
reload 
show version 






Step 4: Configure IPsec VPN on R1
1. Create ACL for Interesting Traffic
bash
Copy
Edit
access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
2. Configure IKE Phase 1 (ISAKMP Policy)
bash
Copy
Edit
crypto isakmp policy 10
encryption aes 256
authentication pre-share
group 5
exit
crypto isakmp key vpnpa55 address 10.2.2.2
3. Configure IKE Phase 2 (IPsec Policy)
bash
Copy
Edit
crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
4. Configure Crypto Map
bash
Copy
Edit
crypto map VPN-MAP 10 ipsec-isakmp
description VPN connection to R3
set peer 10.2.2.2
set transform-set VPN-SET
match address 110
exit
5. Apply Crypto Map to Interface
bash
Copy
Edit
interface s0/0/0
crypto map VPN-MAP
exit
write memory
Step 5: Configure IPsec VPN on R3
Repeat the configuration on R3, adjusting the peer address.

1. Create ACL for Interesting Traffic
bash
Copy
Edit
access-list 110 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255
2. Configure IKE Phase 1 (ISAKMP Policy)
bash
Copy
Edit
crypto isakmp policy 10
encryption aes 256
authentication pre-share
group 5
exit
crypto isakmp key vpnpa55 address 10.1.1.2
3. Configure IKE Phase 2 (IPsec Policy)
bash
Copy
Edit
crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac
4. Configure Crypto Map
bash
Copy
Edit
crypto map VPN-MAP 10 ipsec-isakmp
description VPN connection to R1
set peer 10.1.1.2
set transform-set VPN-SET
match address 110
exit
5. Apply Crypto Map to Interface
bash
Copy
Edit
interface s0/0/1
crypto map VPN-MAP
exit
write memory
Step 6: Verify VPN Tunnel
1. Check IPsec SA before traffic
On R1:

bash
Copy
Edit
show crypto ipsec sa
You should see 0 packets encrypted/decrypted.

2. Generate Interesting Traffic
On PC-A, ping PC-C:

bash
Copy
Edit
ping 192.168.3.3
3. Verify IPsec Tunnel
On R1:

bash
Copy
Edit
show crypto ipsec sa
You should now see packets encrypted and decrypted, confirming VPN is working.

4. Test Uninteresting Traffic
On PC-A, ping PC-B:

bash
Copy
Edit
ping 192.168.2.3
VPN should not be triggered.

5. Confirm VPN Tunnel Behavior
On R1:

bash
Copy
Edit
show crypto ipsec sa
The packet count should remain the same (VPN is not used for uninteresting traffic).

Step 7: Save and Validate
bash
Copy
Edit
write memory
Click Check Results in Packet Tracer to verify 100% completion.

