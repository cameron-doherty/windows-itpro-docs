---
title: SMB v1 Microsoft network server Digitally sign communications (always) (Windows 10)
description: Best practices, security considerations, and more for the  security policy setting, Microsoft network server Digitally sign communications (always).
ms.assetid: 2007b622-7bc2-44e8-9cf1-d34b62117ea8
ms.reviewer: 
ms.author: vinpa
ms.prod: windows-client
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
ms.localizationpriority: medium
author: vinaypamnani-msft
manager: aaroncz
audience: ITPro
ms.topic: conceptual
ms.date: 01/04/2019
ms.technology: itpro-security
---

# SMB v1 Microsoft network server: Digitally sign communications (always)

**Applies to**
-   Windows 10

This topic is about the Server Message Block (SMB) v1 protocol. SMBv1 isn't secure and has been deprecated in Windows. Beginning with Windows 10 Fall Creators Update and Windows Server, version 1709, [SMB v1 isn't installed by default](/windows-server/storage/file-server/troubleshoot/smbv1-not-installed-by-default-in-windows).  

The rest of this topic describes the best practices, location, values, policy management and security considerations for the **Microsoft network server: Digitally sign communications (always)** security policy setting only for SMBv1. The same policy setting can be applied to computers that run SMBv2. Fore more information, see [Microsoft network server: Digitally sign communications (always)](microsoft-network-server-digitally-sign-communications-always.md).

## Reference

The Server Message Block (SMB) protocol provides the basis for file and print sharing and many other networking operations, such as remote Windows administration. To prevent man-in-the-middle attacks that modify SMB packets in transit, the SMB protocol supports the digital signing of SMB packets. 
This policy setting determines whether SMB packet signing must be negotiated before further communication with the Server service is permitted.

Implementation of digital signatures in high-security networks helps to prevent the impersonation of client computers and servers, which is known as "session hijacking." But misuse of these policy settings is a common error that can cause data loss or problems with data access or security.

For this policy to take effect on computers running Windows 2000, client-side packet signing must also be enabled. To enable client-side SMB packet signing, set [Microsoft network client: Digitally sign communications (if server agrees)](smbv1-microsoft-network-client-digitally-sign-communications-if-server-agrees.md). Devices that have this policy set won't be able to communicate with devices that don't have server-side packet signing enabled. By default, server-side packet signing is enabled only on domain controllers. Server-side packet signing can be enabled on devices by setting [Microsoft network server: Digitally sign communications (if client agrees)](smbv1-microsoft-network-server-digitally-sign-communications-if-client-agrees.md).

If server-side SMB signing is required, a client device won't be able to establish a session with that server, unless it has client-side SMB signing enabled. By default, client-side SMB signing is enabled on workstations, servers, and domain controllers. Similarly, if client-side SMB signing is required, that client device won't be able to establish a session with servers that don't have packet signing enabled. By default, server-side SMB signing is enabled only on domain controllers.

If server-side SMB signing is enabled, SMB packet signing will be negotiated with client devices that have SMB signing enabled.

[!INCLUDE [smb1-perf-note](includes/smb1-perf-note.md)]

There are three other policy settings that relate to packet-signing requirements for Server Message Block (SMB) communications:

-   [Microsoft network client: Digitally sign communications (always)](smbv1-microsoft-network-client-digitally-sign-communications-always.md)
-   [Microsoft network client: Digitally sign communications (if server agrees)](smbv1-microsoft-network-client-digitally-sign-communications-if-server-agrees.md)
-   [Microsoft network server: Digitally sign communications (if client agrees)](smbv1-microsoft-network-server-digitally-sign-communications-if-client-agrees.md)

### Possible values

-   Enabled
-   Disabled
-   Not defined

### Best practices

1.  Configure the following security policy settings as follows:

    -   Disable [Microsoft network client: Digitally sign communications (always)](smbv1-microsoft-network-client-digitally-sign-communications-always.md).
    -   Disable **Microsoft network server: Digitally sign communications (always)**.
    -   Enable [Microsoft network client: Digitally sign communications (if server agrees)](smbv1-microsoft-network-client-digitally-sign-communications-if-server-agrees.md).
    -   Enable [Microsoft network server: Digitally sign communications (if client agrees)](smbv1-microsoft-network-server-digitally-sign-communications-if-client-agrees.md).

2.  Alternately, you can set all of these policy settings to Enabled, but enabling them can cause slower performance on client devices and prevent them from communicating with legacy SMB applications and operating systems.

### Location

Computer Configuration\\Windows Settings\\Security Settings\\Local Policies\\Security Options

### Default values

The following table lists the actual and effective default values for this policy. Default values are also listed on the policy’s property page.

| Server type or GPO | Default value |
| - | - |
| Default Domain Policy| Not defined|
| Default Domain Controller Policy | Enabled| 
| Stand-Alone Server Default Settings | Not defined| 
| DC Effective Default Settings | Enabled| 
| Member Server Effective Default Settings| Not defined| 
| Client Computer Effective Default Settings | Disabled| 
 
## Policy management

This section describes features and tools that are available to help you manage this policy.

### Restart requirement

None. Changes to this policy become effective without a device restart when they're saved locally or distributed through Group Policy.

## Security considerations

This section describes how an attacker might exploit a feature or its configuration, how to implement the countermeasure, and the possible negative consequences of countermeasure implementation.

### Vulnerability

Session hijacking uses tools that allow attackers who have access to the same network as the client device or server to interrupt, end, or steal a session in progress. Attackers can potentially intercept and modify unsigned Server Message Block (SMB) packets and then modify the traffic and forward it so that the server might perform objectionable actions. Alternatively, the attacker could pose as the server or client device after legitimate authentication and gain unauthorized access to data.

SMB is the resource-sharing protocol that is supported by many Windows operating systems. It's the basis of NetBIOS and many other protocols. SMB signatures authenticate users and the servers that host the data. If either side fails the authentication process, data transmission doesn't take place.

### Countermeasure

Configure the settings as follows:

-   Disable [Microsoft network client: Digitally sign communications (always)](smbv1-microsoft-network-client-digitally-sign-communications-always.md).
-   Disable **Microsoft network server: Digitally sign communications (always)**.
-   Enable [Microsoft network client: Digitally sign communications (if server agrees)](smbv1-microsoft-network-client-digitally-sign-communications-if-server-agrees.md).
-   Enable [Microsoft network server: Digitally sign communications (if client agrees)](smbv1-microsoft-network-server-digitally-sign-communications-if-client-agrees.md).

In highly secure environments, we recommend that you configure all of these settings to Enabled. However, that configuration may cause slower performance on client devices and prevent communications with earlier SMB applications and operating systems.

>**Note:**  An alternative countermeasure that could protect all network traffic is to implement digital signatures with IPsec. There are hardware-based accelerators for IPsec encryption and signing that could be used to minimize the performance impact on the servers' CPUs. No such accelerators are available for SMB signing.
 
### Potential impact

Implementations of the SMB file and print-sharing protocol support mutual authentication. This mutual authentication prevents session hijacking attacks and supports message authentication to prevent man-in-the-middle attacks. SMB signing provides this authentication by placing a digital signature into each SMB, which is then verified by the client and the server.

Implementation of SMB signing may negatively affect performance because each packet must be signed and verified. If these settings are enabled on a server that is performing multiple roles, such as a small business server that is serving as a domain controller, file server, print server, and application server, performance may be substantially slowed. Additionally, if you configure computers to ignore all unsigned SMB communications, older applications and operating systems can't connect. However, if you completely disable all SMB signing, devices are vulnerable to session-hijacking attacks.

## Related topics

- [Security Options](security-options.md)
