# Active Directory Domain Services Deployment and Windows 11 Domain Join

**Windows Server 2019 | Active Directory | DNS | Windows 11 | VMware Workstation Pro**

## Project Overview

I built a Windows domain lab to demonstrate how centralized identity management can be deployed in a small business environment. I installed Active Directory Domain Services (AD DS) on Windows Server 2019, promoted the server to a domain controller, and joined a Windows 10 and 11 workstations to the domain.

This project created the foundation for centralized authentication, user and computer administration, Group Policy, and controlled access to organizational resources.

## Simulated Business Scenario

A small organization is managing its Windows computers with separate local accounts. This makes user administration, access control, and security policy enforcement difficult to manage consistently.

To address this, I deployed an Active Directory domain that allows administrators to centrally manage identities, computers, policies, and access to shared resources.

## Project Objectives

- Install the AD DS server role on Windows Server 2019.
- Promote the server to a domain controller.
- Create a new Active Directory forest and domain.
- Confirm network connectivity and DNS requirements for domain discovery.
- Join a Windows 11 workstation to the domain.
- Confirm that the workstation successfully accepted the domain membership change.

## Lab Environment

| Component | Technology | Purpose |
| --- | --- | --- |
| Domain controller | Windows Server 2019 | Hosts Active Directory Domain Services and domain DNS services |
| Client workstation | Windows 11 | Represents an end-user computer joined to the domain |
| Directory service | Active Directory Domain Services | Centralizes identity and computer administration |
| Name resolution | DNS | Allows the client to locate the domain controller and domain services |
| Virtualization | VMware Workstation Pro | Hosts the isolated Windows lab environment |

## Skills Demonstrated

- Windows Server administration
- Active Directory Domain Services deployment
- Domain controller promotion
- Active Directory forest and domain creation
- Windows client domain integration
- DNS and network-connectivity verification
- Domain-join troubleshooting
- Technical documentation

## Implementation Summary

### 1. Installed Active Directory Domain Services

Using Server Manager, I added the **Active Directory Domain Services** role and its required management tools to Windows Server 2019. I reviewed the installation selections and completed the role installation successfully.

<p align="center">
  <img src="https://i.imgur.com/FT1I3Y9.png" width="750" alt="Selecting the Active Directory Domain Services role in Server Manager">
</p>

### 2. Promoted the Server to a Domain Controller

After installing AD DS, I promoted the server to a domain controller. I created a new forest, configured the root domain, set the Directory Services Restore Mode password, reviewed the configuration, and completed the prerequisite check before installation.


<p align="center">
  <img src="https://i.imgur.com/aO1BRmT.png" width="750" alt="Creating a new Active Directory forest">
</p>

<p align="center">
  <img src="https://i.imgur.com/YRMdtzM.png" width="750" alt="Active Directory Domain Services prerequisite check">
</p>

### 3. Prepared the Windows 11 Client

Before joining the client to the domain, I confirmed that the workstation could communicate with the domain controller and use the domain controller for DNS resolution. Correct DNS configuration is essential because Active Directory clients use DNS to locate domain services.

### 4. Joined the Windows 11 Workstation to the Domain

From the Windows 11 system properties, I changed the computer membership from a workgroup to the Active Directory domain. I supplied an authorized domain account when prompted and restarted the workstation to complete the change. 

<p align="center">
  <img src="https://i.imgur.com/xhvfeAA.png" width="750" alt="Joining the Windows 11 workstation to the Active Directory domain">
</p>

<p align="center">
  <img src="https://i.imgur.com/ZbwHKgn.png" width="750" alt="Restart prompt after the domain membership change">
</p>

Finally, a quick run of "<i>ipconfig /all</i>" command on the client's machine shows it has successfully been added to the domain.

<p align="center">
  <img src="https://i.imgur.com/CaM4QC1.png" width="750" alt="Confirmation on the clients machine">
</p>


## Validation

I validated the deployment by confirming that:

- The AD DS role installed without errors.
- The server passed the domain-controller prerequisite check.
- The new forest and domain were created successfully.
- The Windows 11 workstation could locate the domain.
- The domain accepted the authorized join request.
- Windows prompted for a restart to finalize the workstation's domain membership.

## Troubleshooting Reference

| Symptom | Likely cause | Verification or resolution |
| --- | --- | --- |
| The domain cannot be contacted | The client is using the wrong DNS server | Run `ipconfig /all`, point the client's preferred DNS server to the domain controller, and test with `nslookup` |
| The client cannot reach the server | Incorrect IP configuration, virtual network, or firewall settings | Verify the subnet and adapter settings, then test connectivity with `ping` |
| Domain credentials are rejected | Incorrect credentials or insufficient join permissions | Verify the account status and use an account authorized to join computers to the domain |
| The join fails unexpectedly | Time difference between the client and domain controller | Check time synchronization with `w32tm /query /status` |

## Security Considerations

- Administrative and Directory Services Restore Mode credentials should never be exposed in documentation screenshots or repository files.
- Production domain joins should use an account with only the permissions required for the task.
- DNS, time synchronization, and firewall settings should be verified before troubleshooting Active Directory authentication.
- Lab credentials and configurations should never be reused in a production/real environment.

## Key Takeaways

This project reinforced that installing AD DS is only the first part of deploying a Windows domain. The server must also be promoted to a domain controller, and client systems must have working network connectivity, correct DNS configuration, and authorized credentials before they can join the domain successfully.

It also demonstrated how Active Directory provides the centralized identity foundation needed for user administration, Group Policy, security controls, and access to shared organizational resources.

<details>
<summary><strong>View the Complete Implementation Walkthrough</strong></summary>

### Install the AD DS Server Role

1. From **Server Manager**, select **Manage > Add Roles and Features**.

   <p align="center"><img src="https://i.imgur.com/q9Cul5y.png" width="750" alt="Opening Add Roles and Features from Server Manager"></p>

2. Review the **Before You Begin** page and continue.

   <p align="center"><img src="https://i.imgur.com/vELutQY.png" width="750" alt="Add Roles and Features introduction page"></p>

3. Select **Role-based or feature-based installation**.

   <p align="center"><img src="https://i.imgur.com/rwFodZy.png" width="750" alt="Selecting role-based or feature-based installation"></p>

4. Select the Windows Server 2019 system from the server pool.

   <p align="center"><img src="https://i.imgur.com/LSfwOAE.png" width="750" alt="Selecting the destination server"></p>

5. Select **Active Directory Domain Services** and add the required features.

   <p align="center"><img src="https://i.imgur.com/9JmTTWA.png" width="750" alt="Selecting Active Directory Domain Services"></p>

6. Review the available features and continue.

   <p align="center"><img src="https://i.imgur.com/cXeQ0tu.png" width="750" alt="Reviewing Windows Server features"></p>

7. Confirm the selections and install the AD DS role.

   <p align="center"><img src="https://i.imgur.com/xQ0vSv4.png" width="750" alt="Confirming the Active Directory Domain Services installation"></p>

### Promote the Server to a Domain Controller

1. After the role installation completes, select **Promote this server to a domain controller**.

   <p align="center"><img src="https://i.imgur.com/V3d5s9L.png" width="750" alt="Starting the domain controller promotion process"></p>

2. Select **Add a new forest** and enter the root domain name for the lab.

   <p align="center"><img src="https://i.imgur.com/aO1BRmT.png" width="750" alt="Adding a new Active Directory forest"></p>

3. Configure the domain-controller options and set a strong Directory Services Restore Mode password.

   <p align="center"><img src="https://i.imgur.com/rDGoYIr.png" width="750" alt="Configuring domain controller options"></p>

4. Review the generated NetBIOS domain name.

   <p align="center"><img src="https://i.imgur.com/KP6Biwj.png" width="750" alt="Reviewing the NetBIOS domain name"></p>

5. Review the database, log, and SYSVOL paths.

   <p align="center"><img src="https://i.imgur.com/VZcQUZt.png" width="750" alt="Reviewing Active Directory database and SYSVOL paths"></p>

6. Review the domain-controller configuration.

   <p align="center"><img src="https://i.imgur.com/kqb6puh.png" width="750" alt="Reviewing the domain controller configuration"></p>

7. Confirm that the prerequisite check completes successfully, then begin the installation.

   <p align="center"><img src="https://i.imgur.com/FYbDnFe.png" width="750" alt="Completing the domain controller prerequisite check"></p>

8. Allow the installation to finish and the server to restart.

   <p align="center"><img src="https://i.imgur.com/smKeyaI.png" width="750" alt="Installing Active Directory Domain Services configuration"></p>

### Join the Windows 11 Client to the Domain

1. On the Windows 11 client, open **This PC > Properties**.

   <p align="center"><img src="https://i.imgur.com/8s95rlK.png" width="750" alt="Opening Windows 11 system properties"></p>

2. From the **About** page, select **Domain or workgroup**.

   <p align="center"><img src="https://i.imgur.com/bMHWldz.png" width="750" alt="Opening domain or workgroup settings"></p>

3. On the **Computer Name** tab, select **Change**.

   <p align="center"><img src="https://i.imgur.com/3oDGfHr.png" width="750" alt="Changing Windows computer membership"></p>

4. Select **Domain**, enter the Active Directory domain name, and confirm the change.

   <p align="center"><img src="https://i.imgur.com/htjNZB0.png" width="750" alt="Entering the Active Directory domain name"></p>

5. Enter credentials for an account authorized to join computers to the domain.

   <p align="center"><img src="https://i.imgur.com/HrTHI66.png" width="750" alt="Authenticating the Windows domain join"></p>

6. Restart the workstation to complete the domain membership change.

   <p align="center"><img src="https://i.imgur.com/Yq3je2z.png" width="750" alt="Restarting Windows after the domain join"></p>

7. And a quick run of "<i>ipconfig /all</i>" command on the client's machine shows it has successfully been added to the domain.

<p align="center">
  <img src="https://i.imgur.com/CaM4QC1.png" width="750" alt="Restart prompt after the domain membership change">
</p>

</details>

## Related Projects

- [Active Directory Identity Administration](https://github.com/AdeniyiAdesakin/Active-Directory-Implementation)
- [Group Policy Object Implementations](https://github.com/AdeniyiAdesakin/Group-Policy-Object-GPO-implementations)
- [Bulk Active Directory User Provisioning with PowerShell](https://github.com/AdeniyiAdesakin/Import-bulk-Users-from-a-CSV-Spreadsheet-with-PowerShell-)

---

**Author:** [Adeniyi Adesakin](https://github.com/AdeniyiAdesakin)  
**LinkedIn:** [linkedin.com/in/adeniyiadesakin](https://www.linkedin.com/in/adeniyiadesakin/)
