---
layout: doc
h3InToc: true
contributedBy: Floris van der Ploeg
description: Citrix Virtual Apps and Desktops supports various printing solutions. It is essential to understand the available technologies and their benefits and limitations to plan and successfully implement the proper printing solution.
tz_title: Baseline Printing Design
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: Baseline Printing Design

## Overview

Citrix Virtual Apps and Desktops supports various printing solutions. It is essential to understand the available technologies and their benefits and limitations to plan and successfully implement the proper printing solution.

## Decision: Printer Provisioning

The process of creating printers at the start of a Citrix Virtual Apps and Desktops session is called printer provisioning. Multiple approaches are available:

-  **User Added**  
  Allowing users to add printers manually gives them the flexibility to select printers by convenience. The drawback of manually adding network-based printers is that it requires the users to know the network name or path of the printers. There is a chance that the native print driver is not available in the Operating System, and the Citrix Universal Print Driver is not compatible. The user needs to seek administrative assistance in these cases. Manually adding printers is best suited in the following situations:
    -  Users roam between different locations using the same client device (for example, using a laptop or tablet).
    -  Users work at assigned stations or areas whose printer assignments rarely change.
    -  Users have personal desktops with sufficient rights to install necessary printer drivers.
-  **Auto Created**  
  Auto-creation is a form of dynamic provisioning that attempts to create available printers on the client device during the login process. Locally attached printers, and network-based printers, are included when printers are auto-created. Automatically creating all client printers can increase the session login time as it enumerates each printer during the login process.
-  **Session-Based**  
  Session printers are a set of network-based printers that are assigned to users through a Citrix policy at the start of each session.
    -  The policy filters proximity-based session printers on the endpoint device's IP subnet. Citrix recommends using proximity printing in the following situations:  
        -  Users roam between different locations using the same endpoint device (for example, using a laptop or tablet).
        -  When using thin clients that can't connect to network-based printers directly.
    -  Session printers can be assigned using the "Session Printer" or the "Printer Assignments" policy. Use the "Session Printer" policy to set default printers for a site, Active Directory group, or OU. Use the "Printer Assignments" setting to assign a large group of printers to multiple users. If both policies are enabled and configured, the session printers merge into a single list.
-  **Universal Printer**  
    The Citrix Universal Printer is a generic printer object. The VDA auto-creates the printer at the start of a session and does not represent a printing device. When using the Citrix Universal Printer, it is not required to enumerate the available client printers during logon. Skipping enumeration of printers can significantly reduce resource usage and decrease user logon times. By default, the Citrix Universal Printer prints to the client's default printer. Administrators can modify this behavior to allow the user to select any of their compatible local or network-based printers. The Citrix Universal Printer is best suited for the following scenarios:
    -  The user requires access to multiple printers, both local and network-based, which can vary with each session.
    -  The user's login performance is a priority, and the Citrix policy "Wait for printers to be created" is enabled.
    -  The user is working from a Windows-based device or thin client.

  >**Note:**
  >
  >Other options for provisioning printers into a Citrix session are available, such as:
  >
  >-  Active Directory group policy
  >-  "Follow-me" centralized print queue solutions
  >-  Other third-party print management solutions

## Decision: Printer Drivers

Managing print drivers in Citrix Virtual Apps and Desktops can be complicated, especially in large environments with hundreds of printers. In Citrix Virtual Apps and Desktops, several methods are available to assist with print driver management.

-  **User Installed**  
    When adding a printer in a Citrix Virtual Apps and Desktops session, it can happen that the native print driver is not available. The user can manually install the missing printer drivers. Many different print drivers can potentially install on various resources creating inconsistencies within the environment. Troubleshooting printing problems and maintenance of print drivers become very challenging since every hosted resource can have different sets of print drivers installed. To ensure consistency and simplify support and troubleshooting, Citrix does not recommend user-installed drivers.
-  **Automatic Installation**
    When connecting a printer within a Citrix Virtual Apps and Desktops session, the VDA checks whether the required print driver is already present in the operating system. If the print driver is not available, the native print driver, if one exists, is installed automatically. If users roam between multiple endpoints and locations, inconsistencies can occur across sessions since users can access a different VDA every time they connect. When this type of scenario occurs, troubleshooting printing problems and maintenance of print drivers can become challenging. Every VDA can have a different set of print drivers installed. To ensure consistency and simplify support and troubleshooting, Citrix does not recommend using automatically installed drivers.
-  **Universal Print Driver**  
    The Citrix Universal Printer Driver (UPD) is a device-independent print driver designed to support most printers. The Citrix UPD simplifies administration by reducing the number of drivers required on the master image. For auto-created client printers, the driver records the output of the application and sends it, without any modification, to the endpoint device. The endpoint uses local, device-specific drivers to finish printing the job to the printer. The UPD can be used in conjunction with the Citrix Universal Print Server to extend this functionality to network printers.

## Decision: Printer Routing

Print jobs can route along different paths: through a client device or a print server.

-  **Client Device Routing**  
    Client devices with locally attached printers route print jobs directly from the client device to the printer.
-  **Windows Print Server Routing**  
    By default, print jobs sent to auto-created network-based printers route from the user's session to the print server. However, the print job takes a fallback route through the client device when any of the following conditions are true:

    -  The session can't contact the print server
    -  The print server is on a different domain without a trust established
    -  The native print driver is not available within the user's session
-  **Citrix Universal Print Server Routing**  
    Print job routing follows the same process as Windows Print Server Routing. The only difference is that Windows uses the Universal Print Driver between the user's session and the Citrix Universal Print Server.

The specifics with print job routing depend on the printer provisioning method. Auto-created and user-added printers can route print jobs based on the following diagrams:

![Client Device Routing](/en-us/tech-zone/design/media/design-decisions_baseline-printing-design_client-device-routing.png)

![Windows Print Server Routing](/en-us/tech-zone/design/media/design-decisions_baseline-printing-design_windows-print-server-routing.png)

![Citrix Universal Print Server Routing](/en-us/tech-zone/design/media/design-decisions_baseline-printing-design_universal-print-server-routing.png)

However, the print job routing changes slightly if the VDA provisions printers as session printers. The jobs can no longer route through the user's endpoint device and route from the session to the print server.

![Session Printers: Windows Print Server Routing](/en-us/tech-zone/design/media/design-decisions_baseline-printing-design_session-printers-windows-print-server-routing.png)

![Session Printers: Citrix Universal Print Server Routing](/en-us/tech-zone/design/media/design-decisions_baseline-printing-design_session-printers-universal-print-server-routing.png)

Citrix bases the recommended option on the network location of the endpoint device, the user's session, and the print server.

-  **Client Device Routing**
    -  Use for locally attached printer implementations.
    -  Use if a Windows endpoint device and printer are on the same high-speed, low-latency network as the Windows Print Server.
-  **Windows Print Server Routing**
    -  Use if the printer is on the same high-speed, low-latency network as the Windows Print Server and user session.
-  **Citrix Universal Print Server Routing**
    -  Use if non-Windows endpoint device and printer are on the same high-speed, low-latency network as the Windows Print Server.

## Decision: Print Server Redundancy

Configure redundancy when managing network-based printers with a Citrix Universal or Windows print server. Redundancy eliminates the print server being a single point of failure. Define the Citrix Universal Print Server redundancy within a Citrix Policy.

  >## Experience from the field
  >
  >A print media company uses thin clients and Windows-based workstations at the company headquarters. The company placed network-based printers throughout the building (one per floor). Windows print servers reside in the data center and manage the network printers. The company hosts the Citrix Virtual Apps and Desktops solution in the data center as well.
  >
  >A regional office has numerous Windows, Linux, and Mac endpoints with network-attached printers. A remote branch office has a few Windows workstations with locally attached printers.
  >
  >The print media company applied three different print strategies:
  >
  >-  **Headquarters**  
  >    Headquarter users use a Citrix Universal Print Server for printing within the Citrix Virtual Apps and Desktops session. The Windows-based workstations do not require native print drivers. A session printer policy is configured per floor, which connects the floor printer as the default printer. The policies are filtered based on the subnet of the thin client for proximity printing.
  >
  >    Network staff implemented Quality of Service (QoS) policies. The QoS policies prioritize inbound, and outbound network traffic on ports TCP 1494, and TCP 2598 over all other network traffic. The QoS policies prevent large print jobs from impacting HDX user sessions.
  >-  **Regional Office**  
  >    The regional office deployed a Universal Print Server. The print job uses the Universal Print Driver and is compressed and delivered from the user's session to the Universal Print Server, across the WAN. The print job then routes to the network-attached printer in the office.
  >-  **Branch Office**  
  >    Since all branch users work on Windows-based workstations, branch office users use auto-created client printers with the Citrix Universal Printer Driver. Since the print job routes over ICA, the print data is compressed, which saves bandwidth. The Citrix Universal Printer Driver guarantees the ability to use all client-connected printers within the user's HDX session without concern about the printer model.
