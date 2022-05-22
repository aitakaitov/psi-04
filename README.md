# psi-04


# Router R1 Configuration

## Interface gigabitEthernet 0/0 (R1 <-> NAT)

<code>configure terminal</code>

<code>interface gigabitEthernet 0/0</code>

<code>ip address dhcp</code>

<code>ip nat outside</code>

<code>no shutdown</code>

<code>exit</code>

## Interface gigabitEthernet 1/0 (R2 <-> R1)

<code>configure terminal</code>

<code>interface gigabitEthernet 1/0</code>

<code>ip nat inside</code>

<code>ip address 192.168.123.253 255.255.255.252</code> (.252 for R1, R2, broadcast and network address)

<code>no shutdown</code>

<code>exit</code>

## Route and NAT

### Route

<code>ip route 192.168.123.0 255.255.255.0 gigabitEthernet 1/0</code>

### NAT

<code>access-list 1 permit 192.168.123.0 0.0.0.255</code>

<code>ip nat inside source list 1 interface gigabitEthernet 0/0 overload</code>


# Router R2 Configuration

## Interface gigabitEthernet 0/0 (R2 <-> R1)

<code>interface gigabitEthernet 0/0</code>

<code>ip address 192.168.123.254 255.255.255.252</code>
  
<code>no shutdown</code>
  
<code>exit</code>

## Interface gigabitEthernet 1/0 (Switch1 <-> R2)

<code>interface gigabitEthernet 1/0</code>

<code>ip address 192.168.123.1 255.255.255.224</code>

<code>no shutdown</code>

<code>exit</code>

## Route

<code>ip route 0.0.0.0 0.0.0.0 192.168.123.253</code> (by default foute to R1)

## DHCP

<code>ip dhcp excluded-address 192.168.123.1</code> (exclude intf 0/0 address from DHCP pool)
  
<code>ip dhcp pool LAN</code>
  
<code>network 192.168.123.0 255.255.255.224</code> (actually 32 addresses incl. broadcast and network - we could exclude 10 more addresses from DHCP pool to make it 20 but it seems unnecessary)
  
<code>default-router 192.168.123.1</code>
  
<code>dns-server 1.1.1.1</code>
  
<code>exit</code>

# PC1 and PC2

Restart them, for some reason DHCP setup works only after restart

Then ping 8.8.8.8 and it works
  
