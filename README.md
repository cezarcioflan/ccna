# ccna

CONFIGURARE SWITCH

en
clo s HH:MM:00 16 May 2022
conf t
no ip domain-look
hostn SwACC1
no cdp run
serv password-enc
enab sec ciscosecpa55
enab pass ciscoenapa55
bann motd *Vineri, ora 14:00 serverul e inchis*

lin con 0
pass ciscoconpa55
login 
logg synch
exec-time 20 20
exi

lin vty 0 15
pass ciscovtypa55
login
logg synch
exec-time 20 20
exi

ip domain-n cti.ro
usern Admin01 priv 15 sec Admin01pa55
lin vty 0 15 
tr in ssh
login loc
exi
spanning-tr m rapid-pvst
cr k gen rsa 
2048

logg host IP_SERVER
serv time log date ms
serv time deb date ms

vlan NO_VLAN
na NAME
vlan NO_MAN
na MAN
vlan NO_NULL
na NULL

int r RANGE
sw m acc
sw acc vlan NO_VLAN  // vlan, null
//null
shut
//

int r RANGE // tot fara man
sw port-sec
sw port-sec max 2
sw port-sec mac-add st
sw port-sec viol shut
spanning-tr bpdu en
spanning-tr portf

//Distr, core
int r RANGE // tot fara man
sw m acc
sw acc vlan NO_VLAN_NULL  
shut
sw port-sec
sw port-sec max 2
sw port-sec mac-add st
sw port-sec viol shut
spanning-tr bpdu en
spanning-tr portf

int r RANGE // man
sw m tr
sw tr nat vlan NO_MAN
sw tr all vlan NO_VLANS // fara null
ex

ip default-gate IP_RVLAN
int vlan NO_MAN
ip add IP MASK
no shut
end
copy run start




CONFIGURARE ROUTER

enable
clock set HH:MM:00 16 May 2022
conf t
no ip domain-lookup
hostname RVLAN
no cdp run
service password-encryption
security passwords min-length 10
login block-for 60 attempts 3 within 25
enable secret ciscosecpa55
enable password ciscoenapa55
banner login #ACCES INTERZIS PERSONALULUI NEAUTORIZAT#
banner motd *Vineri la ora 14:00 serverul se inchide*


line console 0
password ciscoconpa55
login 
logging synchronous
exec-timeout 25 25
exit

line vty 0 15
password ciscovtypa55
login
logging synchronous
exec-timeout 25 25
exit

ip domain-name cti.ro
username Admin01 privilege 15 secret Admin01pa55
line vty 0 15 
transport input ssh
login local
exit
crypto key generate rsa 
2048

logging host IP_SERVER
service timestamps log datetime msec
service timestamps debug datetime msec

interface giga0/0.NO_VLAN // +MAN
description Legatura cu VLAN NO_VLAN
encapsulation dot1q NO_VLAN
ip address IP MASK
exit

interface giga 0/0
--
description Leg cu ramura 
--
ip helper-address ipRserver
no shutdown
exit

interface serial 0/0/0 sau 0/0/1
description leg cu router-ul RCEVA
ip address ip masca
no shutdown
end 
copy run start

////////////////////////////////////////////////

ROUTING

STATIC
ip route N.A. MASK INTERFATA

RIP
router rip
version 2
no auto-summary
network N.A. // echipamente direct conectate
passive-interface  // interfete fara legaturi


EIGPR
router eigrp 1
network N.A. WILDCARD
passive-interface


OSPF
router ospf 1
passive-interface
network N.A. WILDCARD area 0

////////////////////////////////////////////////

DHCP

ip dhcp excluded-address IPmin IPmax
ip dhcp pool NAME
network N.A. MASK
default-router IP_DG
dns-server IPserver

////////////////////////////////////////////////

WIFI

straight thru giga - internet
