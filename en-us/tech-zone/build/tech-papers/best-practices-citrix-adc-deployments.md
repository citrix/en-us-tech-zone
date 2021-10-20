---
layout: doc
h3InToc: true
contributedBy: Steven Wright
description: Tech Paper focused on the steps that a Citrix ADC administrator should follow to deploy a new ADC instance with best practice settings.
tz_title: Best practices for Citrix ADC Deployments
tz_products: citrix-networking
---
# Tech Paper: Best practices for Citrix ADC Deployments

## Overview

This Tech Paper aims to convey what someone skilled in ADC would configure as a generic implementation.

>**Note:** It is unlikely that there is a single configuration that suits everyone. A consultant or administrator with a better understanding of your needs may deviate from these defaults and document the reasons for such changes.

## Power and lights out management settings

If you have purchased a Citrix ADC appliance, you should ensure:

1.  The Citrix ADCs are deployed in locations separate enough to meet your high availability requirements.

1.  The redundant Power Supply Units on each ADC (if you have purchased such) are connected to separate electrical supplies.

1.  The lights out management card (if you have purchased appliances with such a card) are configured.

[You can find more information on configuring lights out management cards here.](/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom.html)

## Physical network cabling, VLANs, and connectivity

### 1. All physical interfaces connecting the Citrix ADC to your networks are redundant

To ensure data flow during a cable, switch, or interface failure, connect your ADC to each network with redundant cables.

To combine the interfaces connecting each network into a single link (known as a channel), you must configure link aggregation on your ADC. When possible, it is preferable to use the Link Aggregation Control Protocol (LACP). However, manual aggregated links are also possible if your network switches do not support LACP.

[Instructions for configuring Link Aggregation on your Citrix ADC can be found here](/en-us/citrix-adc/13/networking/interfaces/configuring-link-aggregation.html)

In a virtualized or cloud environment, your provider handles interface redundancy, and this step is not required.

### 2. Unused physical interfaces are disabled

Disable any physical interfaces that you are not using. Disabling unused interfaces prevents them from being connected to other networks or devices accidentally or maliciously.

You can disable a physical interface by selecting **System, Network, Interfaces**. Then, by ticking the box next to the interface, clicking "**Select Action**", followed by "**Disable**".

### 3. Set all redundant physical interfaces to be unmonitored for HA within System, Network, Interfaces

As each network has redundant connections, it is not desirable that the ADC initiates an HA failover when a single link fails. Instead, the ADC should rely on the remaining link and only trigger a failover if all links fail.

To prevent an HA failover when a single interface in a redundant channel fails, mark the component interfaces as unmonitored.

To mark an interface as unmonitored, select **System, Network, Interfaces**. Then, select each interface that forms a part of each redundant channel and set the "**HA Monitoring**" radio button to "**OFF**".

In a virtualized or Cloud environment, you have virtual interfaces and don't need to perform this step as your provider handles interface redundancy.

### 4. All channels comprising redundant physical interfaces should be monitored for HA within System, Network, Channels

The failure of all aggregated links connecting the ADC to a particular network causes the channel representing those links to enter a failed/DOWN state.

Check that monitoring has been enabled for the channel and the ADC will failover if all redundant links fail.

To mark a channel as monitored, select **System, Network, Channels**. Then, select each channel and set the "**HA Monitoring**" radio button to "**ON**".

### 5. All channels are bound to a separate VLAN and, you have taken care that no untagged channels are accidentally still in VLAN 1

Each redundant channel ordinarily represents the aggregate physical links connecting the ADC to a particular logical network.

In a virtualized or Cloud environment, each interface likely represents aggregate physical links with your provider having completed the aggregation work for you.

By default, the ADC considers all interfaces, channels, and IP addresses as being VLAN 1 and the same logical network. As such, overlooking the VLAN configuration would cause all IP address assigned to the ADC to be available from every directly connected network.

To prevent this behavior, configure VLANs on the ADC to represent your logic networks and appropriately isolate traffic.

[You can find instructions to create VLANs here.](/en-us/citrix-adc/current-release/networking/interfaces/configuring-vlans.html)

### 6. Create an HA pair between your ADCs within System, High Availability

Citrix considers it a best practice to deploy ADCs redundantly. You can achieve redundancy by implementing an HA pair, creating a cluster, or using a technology such as GSLB to split requests between instances. HA pairs comprise two ADC nodes, and clusters can have up to 32-nodes. For a generic implementation, Citrix recommends the creation of a two-node HA pair.

[You can find instructions for configuring an HA pair here](/en-us/citrix-adc/current-release/getting-started-with-citrix-adc/configure-ha-first-time.html)

### 7. Create and bind one SNIP to every VLAN, ensuring that each SNIP is in the subnet of the connected network

Citrix ADC initiates communication from a Subnet IP (called a SNIP) with limited exceptions.

Create one Subnet IP/SNIP for every directly connected logical network. As you have already isolated each network using VLANs, you must bind each SNIP to its respective VLAN. Take care to ensure that no VLANs are missing a SNIP.

The ADC identifies which VLAN each Virtual IP (VIP) needs to be placed in based on the SNIP's subnets. The VLAN configuration then isolates virtual servers within their intended network.

[You can find instructions for configuring SNIPs here.](/en-us/citrix-adc/current-release/networking/ip-addressing/configuring-citrix-adc-owned-ip-addresses/configuring-subnet-ip-addresses-snips.html)

>**Note:** SNIPs host management services by default. To create SNIPs without the management service enabled, append the "-mgmtAccess DISABLED" parameter to the "add ns IP" command.

### 8. Configure the routes that the ADC requires within System, Network, Routes

If you have connected multiple logical networks, you likely have routers in each. Therefore, you must now configure all the routes that the ADC requires to reach its clients and back-end servers.

[You can find instructions for configuring routes here.](/en-us/citrix-adc/current-release/networking/ip-routing/configuring-static-routes.html)

>**Note:** The ADC has a single routing table that applies to all interfaces.

### 9. Create any Policy Based Routes required

Occasionally, configuring a static route to provide for the behavior you desire is impossible.

An ADC with distinct ingress, egress, and dedicated management networks, as well as management clients on the egress network, is the most frequent example.

Static rules would not suffice in this case. Instead, a Policy-Based Route would be required (PBR). By using a PBR, you can force traffic from the ADC's management IP to travel through the management router. Using a PBR will bypass the static routing table, which would otherwise send data to the egress network.

[You can find instructions for configuring Policy Based Routes here.](/en-us/citrix-adc/current-release/networking/ip-routing/configuring-policy-based-routes/configuring-policy-based-routes-pbrs-for-ipv4-traffic.html)

However, if you have the scenario described in our example, you need the following PBR:

```
add ns pbr Management ALLOW -srcIP = <NSIP_of_first_HA_node>-<NSIP_of_second_HA_node> -destIP "!=" <first_IP_management_subnet>–<last_IP_management_subnet> -nextHop <management_subnet_router> - priority 1
apply pbrs
```

### 10. Make sure you can ping each SNIP with Mac Based Forwarding (MBF) is disabled or that you understand why you cannot

Citrix ADC has a mode called Mac Based Forwarding (MBF). MBF causes the ADC to ignore the routing table and to send replies to the MAC address from which it received traffic.

MBF is of great use where you cannot define your routes. Let's say the ADC has multiple Internet connections and must reply using the Internet router through which the traffic arrived. Here, MBF would cause the ADC to record the source MAC address of each connection and use this as the destination MAC in its reply.

However, having MBF override the routing table can make troubleshooting more complex. With MBF, you cannot understand traffic flows from the ADC's configuration file alone as MBF overrides the routing table, and traffic may not flow as you intended. The result is that MBF, while crucial to some implementations, can also lead to misconfigurations in the supporting network remaining undetected.

Disable MBF to ensure that your routing table and PBRs are correct. After disabling MBF, verify that each SNIP remains reachable or that you understand why it does not.

The command to disable MBF is

```
disable ns mode mbf
```

The command to enable MBF is

```
enable ns mode mbf
```

If you do not require MBF, leave it disabled.

[You can find more information of Mac Based Forwarding here.](https://support.citrix.com/article/CTX132952)

### 11. You have installed a new SSL certificate and key for the management GUI within Traffic Management, SSL, Certificates

\
Web browsers do not trust the Citrix ADC's default SSL certificate. The lack of trust causes browsers to display a warning message when accessing the ADC's management services.

Browser warnings for certificates are intended to alert users when the connection is not secure. Citrix recommends that you replace the default SSL certificate so that management users do not become accustomed to accepting warning messages.

[You can find details of how to replace the management SSL certificate here.](https://support.citrix.com/article/CTX122521)

>**Note:** As Citrix ADC shares the management SSL certificate between HA nodes, your replacement certificate must be trusted for all FQDNs used for management purposes. Citrix recommends using a SAN certificate to include the FQDN of both HA nodes.

## Base configuration settings

### 1. Set the timezone and enable NTP

Having accurate and easily understood timestamps in your log files is vital when troubleshooting or handling a security issue.

First, set the timezone to something that makes sense for you. For example, if you have devices that log to a central syslog server and need to cross-reference data from each, use the same timezone as your existing servers.

The command to set the timezone is:

```
set ns param -timezone CoordinatedUniversalTime
```

Second, add NTP servers and enable time synchronization using the commands:

```
add ntp server pool.ntp.org
enable ntp sync
```

After enabling time synchronization, view the NTP status to verify correct behavior by using the following command:

```
nsroot@StevensADC-Primary> show ntp status

     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*any.time.nl     85.199.214.99    2 u  113 1024  377   21.138   +0.762   0.654
 Done
 nsroot@StevensADC-Primary>
```

[You can find details of how to the timezone using the ADC's GUI here.](/en-us/citrix-application-delivery-management-software/current-release/manage-system-settings.html)

[You can find details of how to add NTP servers here.](/en-us/citrix-application-delivery-management-software/current-release/configure/configure-ntp-server.html)

### 2. Create a Key Encryption Key

A Key Encryption Key (commonly known as a KEK) is used to encrypt and decrypt credentials that the ADC must store in a reversible form. For example, the ADC must keep its LDAP bind password reversible to retrieve it at authentication time.

From Citrix ADC firmware 13.0–76.31, the ADC automatically creates a KEK. On later firmware, the command returns an error message that you can safely ignore.

On older releases, you can create a KEK with the command:

```
create kek <RANDOMSTRING>
```

### 3. Set a non-default nsroot password

Recent releases of Citrix ADC firmware prompt you to change the default password for the "nsroot" account when you first log in. Recent firmware builds prompt for the password change as the "**-forcePasswordChange**" system parameter is enabled.

For older releases, you must change the "nsroot" password with the command:

```
set system user nsroot -password <NSROOTPASSWORD>
```

### 4. Add an account for ADM with external authentication disabled

\
Ideally, connect all of your Citrix ADCs to ADM for centralized licensing and management. The connection to ADM requires a user name and password which can be created with the following commands:

```
add system user admuser <ADMPASSWORD> -externalAuth DISABLED -timeout 900
bind system user admuser superuser 100
set system user admuser -externalAuth DISABLED
```

[You can find more details about ADM here.](/en-us/citrix-adc/current-release/adm-service-connect.html)

### 5. Restrict non-management applications access to the NSIP and only HTTPS access

Prevent non-management services from accessing the management IP and set the management IP to require secure communication access (HTTPS rather than HTTP).

```
set ns ip NSIP -restrictAccess enabled -gui SECUREONLY
```

[You can find more details on restricting access to the NSIP here.](https://support.citrix.com/article/CTX126736)

### 6. Set a non-default RPC node password

Set RPC communication (used for HA and GSLB) to use a non-default password.

```
set rpcNode <NSIP_OF_SECONDARY_NODE> -password <RPC_SECONDARY_PASSWORD> -secure YES

set rpcNode <NSIP_OF_PRIMARY_NODE> -password <RPC_PRIMARY_PASSWORD> -secure YES
```

### 7. HA failsafe mode is enabled to ensure that the last healthy node continues to provide service

If an ADC's monitored HA interfaces or routes indicate an error, the ADC enters an unhealthy state and trigger an HA failover. If the second HA node enters an unhealthy state, both stop providing service.

HA fail-safe mode ensures that the last surviving node of a pair continues attempting to provide business services.

```
set HA node -failSafe ON
```

[You can find more details on HA fail-safe mode here.](/en-us/citrix-adc/current-release/system/high-availability-introduction/configuring-fail-safe-high-availability.html)

### 8. Restrict HA failovers to 3 in 1200 seconds

In the unlikely event that HA failovers repeatedly occur, there comes the point where you want them to stop and to attempt to provide service.

Here, we define a limit of three HA failovers within a 1200 second (20 minutes) period.

```
set ha node -maxFlips 3
set ha node -maxFlipTime 1200
```

### 9. Disable SSLv3 for management services

The Citrix ADC management GUI has SSLv3 and TLS1.0 enabled by default. To ensure secure communication, we will disable SSLv3.

```
set ssl service nshttps-::1l-443 -ssl3 disabled
set ssl service nshttps-127.0.0.1-443 -ssl3 disabled
```

Depending on your company's internal security policy, you can optionally disable TLS1.0.

```
set ssl service nshttps-::1l-443 -ssl3 disabled -tls1 disabled
set ssl service nshttps-127.0.0.1-443 -ssl3 disabled -tls1 disabled
```

### 10. Set generic modes and features

Citrix ADC has Layer 3 mode enabled by default. Layer 3 mode causes the ADC to act as a router and can normally be safely disabled. Edge mode causes the ADC to dynamically learn details about back-end servers when used in a configuration such as link load balancing.

```
disable ns mode l3 edge
```

[You can find more details on Layer 3 mode here.](/en-us/citrix-adc/current-release/getting-started-with-citrix-adc/configure-system-settings/configure-modes-packet-forwarding.html)

The specific modes and features that you need depend on your use case. However, we can select a list of options that would apply to most installations.

```
enable ns feature lb ssl rewrite responder cmp
```

[You can find more details about modes and features here.](https://support.citrix.com/article/CTX122942)

### 11. Configure one or more DNS nameserver

The Citrix ADC needs to have access to one or more nameservers for DNS resolution. Citrix ADC checks if the DNS servers are online using an ICMP monitor. To use DNS based monitoring and distribute load, it is usual to implement a local DNS load balancing virtual server.

As DNS can use UDP or TCP, we create one load balancing virtual server for each protocol.

Configure nameservers using the following commands:

```
add lb virtual server DNS_UDP DNS 0.0.0.0 0 -persistenceType NONE -cltTimeout 120

add serviceGroup DNS_UDP_SVG DNS -maxClient 0 -maxReq 0 -cip DISABLED -usip NO -useproxyport NO -cltTimeout 120 -svrTimeout 120 -CKA NO -TCPB NO -CMP NO
bind lb virtual server DNS_UDP DNS_UDP_SVG

add lb monitor DNS_UDP_monitor DNS -query . -queryType Address -LRTM DISABLED -interval 6 -resptimeout 3 -downTime 20 -destPort 53
bind serviceGroup DNS_UDP_SVG -monitorName DNS_UDP_monitor

bind serviceGroup DNS_UDP_SVG <DNSSERVERIP1> 53
bind serviceGroup DNS_UDP_SVG <DNSSERVERIP2> 53


add lb virtual server DNS_TCP DNS_TCP 0.0.0.0 0 -persistenceType NONE -cltTimeout 120

add serviceGroup DNS_TCP_SVG DNS_TCP -maxClient 0 -maxReq 0 -cip DISABLED -usip NO -useproxyport NO -cltTimeout 120 -svrTimeout 120 -CKA NO -TCPB NO -CMP NO
bind lb virtual server DNS_TCP DNS_TCP_SVG

add lb monitor DNS_TCP_monitor DNS-TCP -query . -queryType Address -LRTM DISABLED -interval 6 -resptimeout 3 -downTime 20 -destPort 53
bind serviceGroup DNS_TCP_SVG -monitorName DNS_TCP_monitor

bind serviceGroup DNS_TCP_SVG <DNSSERVERIP1> 53
bind serviceGroup DNS_TCP_SVG <DNSSERVERIP2> 53

add dns nameServer DNS_UDP -type UDP
add dns nameServer DNS_TCP -type TCP
```

### 12. Set TCP and HTTP parameters

While Window Scaling (WS) and Selective Acknowledgment (SACK) now default to enabled in ADC 13.0 firmware, you must enable these TCP settings in earlier firmware versions.

```
set ns tcpparam -WS ENABLED
set ns tcpparam -SACK ENABLED
```

Nagle causes the Citrix ADC to combine data to send a smaller number of larger packets and can be enabled with the following command.

```
set ns tcpparam -nagle ENABLED
```

By default, Citrix ADC forwards HTTP requests that arrive at a load balancer but do not conform to the RFC standard. Configure the ADC to drop invalid requests as a default.

To allow exceptions to the default, you can change the HTTP options on an individual virtual server after discussions with your security team.

Disable support for the HTTP/0.9 protocol, which has been obsolete for over 20 years. For reference, Mosaic 2.0 on Windows 3.1 includes support for HTTP/1.0.

```
set ns httpparam -dropInvalReqs ENABLED -markHttp09Inval ON
```

Cookie version 0 includes absolute timestamps, whereas version 1 cookies have a relative time. Cookies with an absolute timestamp do not expire at the expected time if the client and ADC clock differ. However, cookies with a relative time of +X minutes until expiry will.

Internet Explorer 2 included support for version 1 cookies in 1995, and you are unlikely to experience issues by enabling this option.

```
set ns param -cookieversion 1
```

### 13. Restrict SNMP queries to select servers

It is good practice that the ADC only answers SNMP queries from hosts supposed to make such queries. Limit the hosts that the ADC allows SNMP queries from using the command:

```
set snmp manager SNMPMANAGERIP
```

[You can find more details on configuring SNMP here.](/en-us/citrix-adc/current-release/getting-started-with-citrix-adc/configure-system-settings/configure-snmp.html)

### 14. Set SNMP alarms and traps

It is helpful for the ADC to raise alerts when high CPU or memory usage conditions occur. You can send alerts via trap configuration to your SNMP server. You may also want to trigger alerts when HA failovers occur. You can implement such configuration with the following commands:

```
set snmp alarm CPU-USAGE -state ENABLED -normalValue 35 -thresholdValue 80 -logging ENABLED -severity Informational
set snmp alarm MEMORY -state ENABLED -normalValue 35 -thresholdValue 80 -logging ENABLED -severity Critical
set snmp alarm HA-STATE-CHANGE -severity Critical

add snmp trap generic SNMPTRAPDSTIP -communityName public
```

>**Note:** Citrix recommends that you consider periodically reviewing threshold values to ensure that the ADC alerts on abnormal behavior that you want to investigate.

### 15. Set a remote syslog server

Audit logging should be configured on an ADC, and audit logs should be stored and analyzed on a remote server.

You can configure the Citrix ADC to send audit logs to a remote Syslog server using the following commands:

```
add audit syslogAction RemoteSyslogServerAction SYSLOGSERVERIP -loglevel ALL
add audit syslogpolicy RemoteSyslogServerPolicy true RemoteSyslogServerAction
bind audit syslogGlobal -policyName RemoteSyslogServerPolicy -priority 100
```

[You can find more details about audit logging here]
(/en-us/citrix-adc/current-release/system/audit-logging.html)

### 16. Set a timeout and prompt for management sessions

Citrix ADC 13.0 allows a default of 900 seconds (15 minutes) before disconnecting idle management sessions. On older firmware versions, you must ensure that you have configured an appropriate timeout.

```
set system parameter -timeout 900
```

An administrator can open SSH sessions to multiple ADCs at the same time. Changing the ADC's CLI prompt helps clarify the node to which a session is connected.

The following commands cause the CLI prompt to display their username, the ADC's host name, and the HA status of the instance.

```
set system parameter -promptString %u@%h-%s
```

After you run the command, the prompt will be rendered as:

```
nsroot@hostname-Primary>
```

### 17. Centralized authentication for management accounts

Security teams generally consider it better to control management accounts from a central platform such as Active Directory than to create accounts on each device. Usually, these centralized accounts are then granted permissions based on group memberships.

The rationale for centralized authentication and authorization is usually that managing accounts on each device is time-consuming and prone to error. Also, there is the risk that management users not change their passwords frequently and that IT can forget to remove ex-employees' accounts.

Use the following commands to configure centralized authentication, keeping in mind that the LDAP filter controls who are allowed to log in.

```
add authentication ldapAction LDAP_mgmt_auth -serverIP <LDAPMANAGEMENTSERVERIP> -serverPort 636 -ldapBase "<dc=mycoolcompany,dc=local>" -ldapBindDn "<serviceaccount@mycoolcompany.local>" -ldapBindDnPassword <LDAPPASSWORD> -ldapLoginName <sAMAccountName> -searchFilter "&(|(memberOf:1.2.840.113556.1.4.1941:<cn=Citrix-ADC-FullAccess,ou=groups,dn=mycoolcompany,dc=local>)(memberOf:1.2.840.113556.1.4.1941:<cn=Citrix-ADC-ReadOnly,ou=groups,dn=mycoolcompany,dc=local>))" -groupAttrName memberOf -subAttributeName cn -secType SSL -passwdChange ENABLED -nestedGroupExtraction ON -maxNestingLevel 5 -groupNameIdentifier samAccountName -groupSearchAttribute memberOf -groupSearchSubAttribute CN

add authentication Policy LDAP_mgmt_pol -rule true -action LDAP_mgmt_auth
bind system global LDAP_mgmt_pol -priority 100
```

While these commands implement authentication, they do not control authorization and, by default, the authenticated users are not able to perform any actions.

To grant the user (or more accurately, the group of which the user is a member) the right to perform actions on the ADC, you should use the following commands:

```
add system group Citrix-ADC-FullAccess -timeout 900
add system group Citrix-ADC-ReadOnly -timeout 900
bind system group Citrix-ADC-FullAccess -policyName superuser 100
bind system group Citrix-ADC-ReadOnly -policyName read-only 110
```

[You can find more information about centralized authentication and authorization here.](https://support.citrix.com/article/CTX123782)

[You can also find information about the LDAP filter string used above here.](https://support.citrix.com/article/CTX201948)

Further, from ADC firmware version 12.1.51.16 you can configure multifactor authentication for management users by [following the steps here.](/en-us/citrix-adc/current-release/system/authentication-and-authorization-for-system-user/two-factor-authentication-for-system-users-and-external-users.html)

### 18. Disable LDAP authentication for the nsroot user

As the Citrix ADC processes authentication and authorization separately, users can authenticate using LDAP, and the ADC grants permissions based on their group membership.

While not a good idea, you could also create user accounts with authorization permissions on the ADC. A password associated with an Active Directory user with the same name could then be used to authenticate these accounts.

To prevent an Active Directory administrator from creating an "nsroot" user and being able to authenticate, you must disable external authentication for the "nsroot" user account.

```
set system user nsroot -externalAuth DISABLED
```

### 19. TLS/SSL Best practices

You can now follow the TLS/SSL Best Practice document to define a secure cipher suite that can be used to protect your virtual servers.

[You can find the TLS/SSL best practice document here.](/en-us/tech-zone/build/tech-papers/networking-tls-best-practices.html)
