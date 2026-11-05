# HOSPITAL NETWORK
## Detailed Network Design and Documentation
**Cisco Packet Tracer Project**

*Network Infrastructure | VLANs | Inter-VLAN Routing | HSRP | DHCP | DNS | HTTP*

---

# Table of Contents

1. Introduction
2. Project Overview
3. Project Objectives
4. Network Topology
5. Building and Department Mapping
6. Network Architecture
7. VLAN Design
8. IP Addressing Plan
9. Devices and Interfaces
10. Inter-VLAN Routing
11. HSRP and High Availability
12. Network Services
13. Network Security Considerations
14. Testing and Verification
15. Troubleshooting and Maintenance
16. Scalability and Future Improvements
17. Conclusion

---

# 1. Introduction

A modern military hospital requires a reliable and secure network infrastructure to connect its administrative, medical, technical, and service departments. The network must enable authorized communication between users and systems while maintaining departmental separation and ensuring access to essential network services.

This project presents the design and implementation of a hospital network using Cisco Packet Tracer. The proposed infrastructure connects four buildings, identified as B1, B2, B3, and B4. It uses Virtual Local Area Networks (VLANs) to separate departments, multilayer switches to provide inter-VLAN routing, and the Hot Standby Router Protocol (HSRP) to improve default gateway availability.

Centralized Dynamic Host Configuration Protocol (DHCP), Domain Name System (DNS), and Hypertext Transfer Protocol (HTTP) services are deployed in Building B4.

This documentation describes the network topology, departmental layout, VLAN configuration, IP addressing scheme, device roles, routing architecture, redundancy mechanisms, network services, security considerations, and testing procedures.

# 2. Project Overview

The proposed Military Hospital Network is designed to connect four buildings containing different hospital departments and network services.

Building B1 accommodates administrative and technical departments, while Buildings B2 and B3 primarily support medical departments. Building B4 serves as the centralized network services location, hosting the DHCP, DNS, and HTTP servers.

Each building is divided into floors, and departments are assigned dedicated VLANs according to their functional requirements. This logical segmentation prevents the entire hospital network from operating as a single broadcast domain.

Multilayer switches provide Layer 3 connectivity between VLANs and serve as departmental default gateways. HSRP provides a virtual default gateway address that can remain available when the active gateway device fails, provided the standby device and its network paths remain operational.

The design establishes a structured, manageable, and expandable network infrastructure suitable for demonstrating enterprise networking concepts in Cisco Packet Tracer.

# 3. Project Objectives

The primary objectives of this project are:

- Design a structured network infrastructure connecting four hospital buildings.
- Organize hospital departments using VLAN segmentation.
- Implement inter-VLAN routing using multilayer switches.
- Configure HSRP to provide redundant default gateways.
- Deploy centralized DHCP, DNS, and HTTP services.
- Develop a consistent IP addressing plan using the `192.168.X.0/24` network structure.
- Enable communication between authorized users, departments, buildings, and centralized servers.
- Apply fundamental network security practices to improve isolation and access control.
- Verify network functionality through connectivity tests and service-specific checks.
- Establish a scalable architecture that can accommodate future departmental and infrastructure expansion.

# 4. Network Topology

The network topology represents the physical and logical connections between the four hospital buildings, multilayer switches, access switches, end devices, wireless access points, IP phones, and centralized servers.

The design separates user connectivity from Layer 3 routing functions. Access switches connect departmental devices, while uplinks provide connectivity to the multilayer switching infrastructure.

VLANs are extended across trunk links where required, allowing devices in the same VLAN to communicate across different switches. Multilayer switches provide the necessary Layer 3 interfaces and route traffic between departmental networks.

Building B4 contains the centralized servers that provide network services to clients throughout the hospital.

The topology is designed to demonstrate the following networking functions:

- Departmental VLAN segmentation.
- Trunk links between switches.
- Inter-VLAN routing through switched virtual interfaces (SVIs).
- Redundant default gateways using HSRP.
- Centralized DHCP, DNS, and HTTP services.
- Connectivity between buildings and departments.

# 5. Building and Department Mapping

The hospital network is divided into four buildings, each containing designated departments and network facilities.

| Building | Floor | Departments / Areas |
|---|---|---|
| B1 | F0 – Ground | Reception, IT, Security |
| B1 | F1 – First | HR, AD |
| B1 | F2 – Second | AR, MT |
| B2 | F0 – Ground | Reception, IT, Emergency, X-Ray, Nursing |
| B2 | F1 – First | Clinic, Lab |
| B2 | F2 – Second | OP, Nursing, ICU |
| B3 | F0 – Ground | Reception, IT, Pharmacy |
| B3 | F1 – First | Clinic, Nursing |
| B3 | F2 – Second | Clinic, PT |
| B4 | F0 – Ground | DHCP, DNS, HTTP servers |

**Floor notation:** F0 represents the ground floor, F1 represents the first floor, and F2 represents the second floor.

This departmental mapping provides a clear relationship between the physical hospital layout and its logical network design. Departments can be assigned to appropriate VLANs regardless of their physical locations.

Building B4 functions as the centralized service location, providing shared network services to authorized clients across all four buildings.

# 6. Network Architecture

The network follows a hierarchical architecture that separates endpoint connectivity, switching, and Layer 3 routing responsibilities.

## 6.1 Access Layer

The access layer consists of switches connected to PCs, laptops, IP phones, printers, wireless access points, and other endpoint devices.

Access ports are assigned to the appropriate departmental VLANs. Where supported and required, IP phone connections can use a separate voice VLAN.

## 6.2 Layer 3 Switching Layer

Multilayer switches provide the network's Layer 3 functionality. They host SVIs for departmental VLANs, perform inter-VLAN routing, and participate in HSRP to provide redundant default gateways.

## 6.3 Inter-Building Connectivity

Uplinks connect the building networks to the wider switching infrastructure. Trunk links carry multiple VLANs where necessary, while routed links may be used where the topology requires Layer 3 connectivity between network devices.

## 6.4 Centralized Services Layer

Building B4 hosts the DHCP, DNS, and HTTP servers. These servers provide shared services to clients throughout the hospital network.

This architecture simplifies troubleshooting, supports centralized service management, and provides a foundation for future expansion.

# 7. VLAN Design

Virtual Local Area Networks (VLANs) logically separate departmental devices into individual broadcast domains. Each VLAN represents a distinct network segment, helping to organize devices according to their operational roles.

VLAN segmentation reduces unnecessary broadcast traffic and simplifies network administration. It also provides a foundation for implementing Access Control Lists (ACLs) and other policies to control communication between departments.

The proposed VLAN allocation is shown below.

| VLAN ID | Department / Function |
|---:|---|
| 10 | Reception |
| 20 | IT |
| 30 | Security |
| 40 | HR |
| 50 | AD |
| 60 | AR |
| 70 | MT |
| 80 | Emergency |
| 90 | X-Ray |
| 100 | Clinic |
| 110 | Lab |
| 120 | ICU |
| 130 | OP |
| 140 | Nursing |
| 160 | Pharmacy |
| 170 | PT |
| 180 | Servers |

VLAN IDs are used to identify logical network segments across the switching infrastructure. The same VLAN can be present on multiple access switches when departments require connectivity across different floors or buildings.

Inter-VLAN communication is handled by the multilayer switches. Additional access-control policies can be applied to restrict communication between sensitive departments and general user networks.

# 8. IP Addressing Plan

The network uses a consistent IPv4 addressing convention in which VLAN ID X corresponds to the network `192.168.X.0/24`.

Each VLAN uses the subnet mask `255.255.255.0`, providing 254 usable host addresses per subnet. The HSRP virtual IP address is assigned as `.1` in each VLAN.

The physical SVI addresses on the multilayer switches must be different from the virtual gateway address. For example, the two switches could use `.2` and `.3`, while clients use `.1` as their default gateway.

| VLAN | Network Address | Subnet Mask | HSRP Virtual Gateway |
|---:|---|---|---|
| 10 | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1 |
| 20 | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1 |
| 30 | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1 |
| 40 | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1 |
| 50 | 192.168.50.0/24 | 255.255.255.0 | 192.168.50.1 |
| 60 | 192.168.60.0/24 | 255.255.255.0 | 192.168.60.1 |
| 70 | 192.168.70.0/24 | 255.255.255.0 | 192.168.70.1 |
| 80 | 192.168.80.0/24 | 255.255.255.0 | 192.168.80.1 |
| 90 | 192.168.90.0/24 | 255.255.255.0 | 192.168.90.1 |
| 100 | 192.168.100.0/24 | 255.255.255.0 | 192.168.100.1 |
| 110 | 192.168.110.0/24 | 255.255.255.0 | 192.168.110.1 |
| 120 | 192.168.120.0/24 | 255.255.255.0 | 192.168.120.1 |
| 130 | 192.168.130.0/24 | 255.255.255.0 | 192.168.130.1 |
| 140 | 192.168.140.0/24 | 255.255.255.0 | 192.168.140.1 |
| 160 | 192.168.160.0/24 | 255.255.255.0 | 192.168.160.1 |
| 170 | 192.168.170.0/24 | 255.255.255.0 | 192.168.170.1 |
| 180 | 192.168.180.0/24 | 255.255.255.0 | 192.168.180.1 |

## Example Client Configuration

A PC connected to VLAN 10 could use the following configuration:

- IP address: `192.168.10.10`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.10.1`
- DNS server: The configured IP address of the DNS server in VLAN 180.

A client connected to VLAN 100 would use an address from `192.168.100.0/24` and the default gateway `192.168.100.1`.

The exact IP addresses assigned to the centralized servers should be documented separately and excluded from the dynamic DHCP address pools to prevent address conflicts.

# 9. Devices and Interfaces

The network uses different device types to provide endpoint connectivity, switching, routing, redundancy, and centralized services.

| Device | Primary Role |
|---|---|
| Routers | Provide routing and external connectivity where required by the topology. |
| Multilayer switches | Provide SVIs, inter-VLAN routing, HSRP, and default gateway functionality. |
| Access switches | Connect endpoint devices and assign access and voice VLANs. |
| PCs | Provide departmental workstation connectivity. |
| Laptops | Support mobile and wireless users. |
| Wireless access points | Provide wireless network access. |
| IP phones | Support hospital voice communications. |
| DHCP server | Automatically assigns IP configuration to clients. |
| DNS server | Resolves hostnames to IP addresses. |
| HTTP server | Hosts the internal hospital web service. |

End devices connect through access ports assigned to the correct VLAN. Switch uplinks use trunking when multiple VLANs must traverse the same connection.

Multilayer switches use SVIs to provide Layer 3 connectivity for the configured VLANs. HSRP is configured on the appropriate gateway interfaces when redundant multilayer switches are available.

All device interface assignments, cable connections, and IP addresses should be recorded in the final topology documentation.

# 10. Inter-VLAN Routing

Inter-VLAN routing enables devices in different VLANs to communicate through Layer 3 routing.

Because VLANs are separate broadcast domains, a device cannot directly communicate with another VLAN using Layer 2 switching alone. Traffic destined for a different subnet must be forwarded to a Layer 3 gateway.

In this project, multilayer switches provide inter-VLAN routing through Switched Virtual Interfaces (SVIs). Each SVI is associated with a VLAN and provides the Layer 3 interface for that network.

For example, when a Reception PC in VLAN 10 communicates with a Clinic PC in VLAN 100, the Reception PC forwards the packet to its default gateway, `192.168.10.1`. The multilayer switching layer routes the packet toward the destination network, `192.168.100.0/24`.

The destination device then receives the packet if the routing and access policies permit the communication.

This design provides centralized routing functionality and enables the network administrator to apply ACLs and other controls to interdepartmental traffic.

# 11. HSRP and High Availability

The Hot Standby Router Protocol (HSRP) provides first-hop gateway redundancy by allowing two or more Layer 3 devices to share a virtual IP address.

In this project, two multilayer switches participate in an HSRP group for each VLAN that requires gateway redundancy. One switch operates as the active gateway, while another operates as the standby gateway.

Clients are configured to use the HSRP virtual IP address as their default gateway instead of relying on the physical address of an individual switch.

If the active gateway becomes unavailable, the standby device can take over the active role and continue forwarding traffic through the virtual gateway.

For example, VLAN 10 uses `192.168.10.1` as its virtual gateway. The participating switches use separate physical SVI addresses, such as `192.168.10.2` and `192.168.10.3`.

HSRP improves gateway availability and reduces dependence on a single Layer 3 device. However, successful failover also depends on the availability of the standby switch, VLAN connectivity, and the remaining network paths.

The implementation should be tested by verifying HSRP states and simulating an active gateway failure.

# 12. Network Services

Building B4 hosts the centralized network services used by clients throughout the hospital.

## 12.1 DHCP Service

The Dynamic Host Configuration Protocol (DHCP) automatically assigns IP configuration to client devices.

A DHCP scope should be configured for each client VLAN, specifying the appropriate network, subnet mask, default gateway, DNS server, and address allocation range.

For example, clients in VLAN 10 receive addresses from the `192.168.10.0/24` network and use `192.168.10.1` as their default gateway.

Because the DHCP server resides in VLAN 180, DHCP relay functionality must be configured on the client VLAN gateway interfaces if clients in other VLANs need to reach the centralized server. On Cisco devices, this is commonly implemented using the `ip helper-address` command.

Server addresses and other statically assigned addresses must be excluded from dynamic allocation.

## 12.2 DNS Service

The Domain Name System (DNS) translates hostnames into IP addresses.

A centralized DNS server allows users to access internal services through meaningful hostnames rather than remembering numerical addresses.

For example, an internal hostname such as `hospital.local` can be configured to resolve to the IP address of the HTTP server, provided the corresponding DNS record is created and clients are configured to use the DNS server.

DNS functionality should be verified by testing hostname resolution from client devices.

## 12.3 HTTP Service

The Hypertext Transfer Protocol (HTTP) service provides access to the hospital's internal web application or demonstration webpage.

The HTTP server can host information such as hospital announcements, internal service details, or a sample hospital homepage.

Authorized clients should be able to access the server using its IP address or a configured DNS hostname, provided routing and security policies permit the connection.

The service can be tested using the web browser available on Cisco Packet Tracer end devices.

# 13. Network Security Considerations

Network security is an important consideration in a hospital environment because different departments may handle sensitive operational and medical information.

The project uses VLAN segmentation to separate departmental broadcast domains. However, VLANs alone do not prevent all communication between departments because Layer 3 routing can allow traffic to pass between VLANs.

The following measures can strengthen the design:

- **Access Control Lists (ACLs):** Restrict communication between departmental VLANs according to approved access requirements.
- **Secure device management:** Use strong credentials and restrict administrative access to authorized management hosts.
- **Unused switch ports:** Administratively disable ports that are not in use.
- **Management network separation:** Use a dedicated management VLAN or network for network administration where appropriate.
- **DHCP security:** Apply appropriate DHCP controls and exclude statically assigned server addresses from dynamic pools.
- **Wireless security:** Use appropriate authentication and encryption for wireless access.
- **Server protection:** Restrict access to centralized servers according to the services each department requires.
- **Monitoring and logging:** Use Syslog and network monitoring to help identify failures and suspicious activity.

In a real military hospital, the design would also require a formal security assessment, approved access policies, secure administrative procedures, and additional safeguards appropriate to the sensitivity of its information systems.

These security features should be implemented and tested according to the project's requirements and the capabilities supported by the selected Packet Tracer devices.

# 14. Testing and Verification

Testing and verification confirm that the implemented network functions according to the design requirements.

The following tests should be performed after completing the configurations.

| Test | Purpose | Expected Result |
|---|---|---|
| PC to default gateway | Verify gateway reachability | Successful response |
| PC to same-VLAN PC | Verify local VLAN connectivity | Successful communication |
| PC to different-VLAN PC | Verify inter-VLAN routing | Successful communication when permitted |
| Building-to-building connectivity | Verify inter-building paths | Successful communication when permitted |
| Client to DHCP server | Verify address assignment | Correct IP configuration |
| Client to DNS server | Verify DNS reachability | Server reachable |
| Client hostname lookup | Verify DNS resolution | Correct IP address returned |
| Client to HTTP server | Verify application connectivity | Hospital webpage accessible |
| HSRP failover | Verify gateway redundancy | Standby assumes the active role after failure |

## 14.1 Connectivity Testing

Use the `ping` command to test connectivity between clients, gateways, and servers.

A successful ping indicates IP reachability between the tested devices, but it does not prove that every network service is functioning correctly.

## 14.2 DHCP Testing

Configure a client to obtain an IP address automatically. Verify that it receives an address from the correct subnet, the correct subnet mask, the HSRP virtual gateway, and the intended DNS server.

## 14.3 DNS and HTTP Testing

Test DNS resolution using a configured hostname and open the HTTP server's webpage through the client browser.

If DNS resolution fails but access by IP address succeeds, investigate the DNS configuration and records.

## 14.4 HSRP Testing

Use the appropriate Cisco IOS commands to verify HSRP operation. Simulate a failure of the active gateway and confirm that the standby switch assumes the active role.

Useful verification commands include:

- `show vlan brief`
- `show interfaces trunk`
- `show ip interface brief`
- `show ip route`
- `show standby brief`

The final report should record the actual results obtained during testing rather than assuming every test has passed.

# 15. Troubleshooting and Maintenance

A systematic troubleshooting approach helps identify network problems efficiently. Troubleshooting should begin with physical connectivity and progress through switching, addressing, routing, and application services.

## 15.1 Physical Connectivity

Check cable connections, interface status, and link indicators. Verify that the relevant interfaces are enabled and operating correctly.

## 15.2 VLAN and Trunk Configuration

Confirm that access ports belong to the correct VLANs. Verify that trunk links are operational and permit all required VLANs.

## 15.3 IP Addressing and Gateway Configuration

Check client IP addresses, subnet masks, default gateways, and DNS settings. Confirm that each client is using an address from the correct VLAN subnet.

## 15.4 Inter-VLAN Routing

Verify that the required VLANs exist, their SVIs are operational, and Layer 3 routing is enabled on the multilayer switches.

Check routing information and confirm that the required destination networks are reachable.

## 15.5 DHCP Troubleshooting

Verify the DHCP scope, address range, subnet mask, gateway, and DNS settings. If the DHCP server is in another VLAN, confirm that the required relay configuration is present.

## 15.6 DNS and HTTP Troubleshooting

For DNS problems, check the client DNS configuration, server status, and hostname records.

For HTTP problems, verify server reachability, HTTP service status, and the correct URL.

## 15.7 HSRP Troubleshooting

Check the active and standby states, virtual IP configuration, HSRP group settings, and Layer 2 connectivity between participating switches.

Regular maintenance should include configuration backups, documentation updates, interface checks, and periodic connectivity testing.

# 16. Scalability and Future Improvements

The proposed architecture provides a foundation for future network expansion.

Additional departments can be introduced by creating new VLANs and assigning corresponding IP subnets. More access switches can be deployed when the number of users or connected devices increases.

The following enhancements could improve the network further:

- **Network monitoring:** Deploy tools such as Zabbix or SolarWinds to monitor device availability and interface performance.
- **Centralized logging:** Introduce Syslog for collecting device logs and supporting troubleshooting.
- **Time synchronization:** Configure Network Time Protocol (NTP) for consistent timestamps across network devices.
- **Traffic monitoring:** Use SNMP to collect interface statistics and device health information.
- **Firewall integration:** Implement firewall policies to control traffic between sensitive departments and external networks.
- **Improved access control:** Introduce more detailed ACL policies based on departmental requirements.
- **Server redundancy:** Provide redundant infrastructure for essential DHCP, DNS, and HTTP services where required.
- **Wireless improvements:** Strengthen wireless authentication, encryption, and network segmentation.
- **Management network:** Separate device administration from ordinary user traffic.
- **Configuration automation:** Explore Python-based automation for configuration backups, device checks, and routine network administration.

For a production environment, the network would also benefit from documented recovery procedures, regular security reviews, and testing of critical service availability.

# 17. Conclusion

The Military Hospital Network project demonstrates the design and implementation of a structured network infrastructure connecting administrative, technical, medical, and centralized service departments across four buildings.

VLAN segmentation organizes departmental devices into logical networks, while multilayer switches provide inter-VLAN routing. HSRP improves default gateway availability by allowing a standby device to assume the active gateway role when necessary.

Centralized DHCP, DNS, and HTTP services support automatic IP configuration, hostname resolution, and internal web access. A consistent IP addressing scheme simplifies network administration, troubleshooting, and future expansion.

The project also demonstrates the importance of network security, systematic testing, and ongoing maintenance. Although the Packet Tracer implementation represents a simplified model of a real hospital network, it provides a practical foundation for understanding enterprise switching, routing, redundancy, and network services.

Future improvements, including firewall integration, centralized monitoring, stronger access controls, and additional service redundancy, could further enhance the reliability, security, and scalability of the proposed infrastructure.
