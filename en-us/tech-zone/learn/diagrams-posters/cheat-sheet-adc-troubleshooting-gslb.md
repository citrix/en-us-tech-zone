---
layout: doc
---
# Troubleshooting Citrix GSLB MEP Cheat Sheet

## Contributors

**Author:** [Gene Whitaker](mailto:gene.whitaker@citrix.com)

Citrix ADC appliances configured for global server load balancing (GSLB) provide for disaster recovery and ensure continuous availability of applications by protecting against points of failure in a wide area network (WAN). GSLB can balance the load across data centers by directing client requests to the closest or best performing data center, or to surviving data centers in case of an outage.

*  GSLB Metric Exchange Protocol (MEP) is a Citrix proprietary protocol used by Citrix ADC to exchange site and network metrics, persistence data, and other information to other sites participating in GSLB
*  Communication process is accomplished between each GSLB site on TCP port 3011 (or 3009 for secure communication).
*  MEP communication is initiated from the lower site IP address to the higher site address for site metrics exchange and higher to lower for network metrics.

The Citrix ADC GSLB and sync cheat sheet provides you with the most commonly used resources to troubleshoot Citrix ADC GSLB and sync issues. Use the following link to [download GSLB and Sync Cheat Sheet](/en-us/tech-zone/learn/downloads/cheat-sheet-adc-troubleshooting-gslb.pdf).

[![TODO](/en-us/tech-zone/learn/media/cheat-sheet-adc-troubleshooting-gslb_1.png)](/en-us/tech-zone/learn/downloads/cheat-sheet-adc-troubleshooting-gslb.pdf)
