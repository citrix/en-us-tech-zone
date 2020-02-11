---
layout: doc
description: Copy & paste description from TOC here
---
# Workspace - Single Sign-On

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

A user’s primary Workspace identity authorizes them to access SaaS, mobile, web, virtual apps and virtual desktops. Many authorized resources require additional authentication, often with an identity different from the user’s primary workspace identity. Citrix Workspace provides users with a seamless experience by providing single sign-on to secondary resources.

## Primary Identity

Understanding the differences between a user’s primary and secondary identities provides a foundation for understanding Workspace single sign-on.

[![Workspace Identity](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)

To start, Citrix Workspace allows each organization to choose a primary identity from a growing list of options, which currently includes:

*  Windows Active Directory
*  Azure Active Directory
*  Okta
*  Citrix Gateway
*  Google

Within Citrix Workspace, the user’s primary identity serves two purposes:

1.  Authenticates the user to Citrix Workspace
1.  Authorizes the user to access a set of resources within Citrix Workspace

Once the user successfully authenticates to Citrix Workspace with the primary identity, they have authorization to all secondary resources. It is critical for organizations to setup strong authentication policies for the user’s primary identity.

Many identity providers include strong authentication policy options, helping to secure the user’s primary authentication to Citrix Workspace.  In cases where the identity provider only includes a single username and password, like Active Directory, Citrix Workspace includes additional capabilities to improve primary authentication security, like Time-based One Time Password.

To gain a deeper understanding of the primary identity for Citrix Workspace, please refer to the Workspace Identity Tech Brief.

## Secondary Identities

