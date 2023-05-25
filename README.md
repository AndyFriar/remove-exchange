## Exchange Management Tools on Server 2019 when no Exchange was built in first place

1. Add .NET 4.8.

2. Run the following command in Command Prompt or PowerShell:

Setup /PrepareAD /OrganizationName:"MyOrganization" /IAcceptExchangeServerLicenseTerms_DiagnosticDataON /role:ManagementTools /InstallWindowsComponents

3. Reboot the server.

4. Fire up PowerShell and execute the following command:
```
Add-PSSnapin Microsoft.Exchange.Management.PowerShell.SnapIn
```

5. Create a new remote domain:
```
New-RemoteDomain -Name 'Hybrid Domain - tenant.mail.onmicrosoft.com' -DomainName 'tenant.mail.onmicrosoft.com'
```

6. Set the remote domain for target delivery:
```
Set-RemoteDomain -TargetDeliveryDomain: $true -Identity 'Hybrid Domain - tenant.mail.onmicrosoft.com'
```

7. Add the Recipient Management snap-in:
```
Add-PSSnapin *RecipientManagement
```

8. Change the directory to the Exchange installation scripts folder:

```
cd $env:ExchangeInstallPath\Scripts\
```

9. Run the script to add permissions for the Exchange Management Tools:
  ```
  Add-PermissionForEMT.ps1
  ```

1.  Run the script to clean up Active Directory Exchange Management Tools:
 ```
 CleanupActiveDirectoryEMT.ps1
 ```

1.  Check the accepted domains:
 ```
 Get-AcceptedDomain
 ```

1.  Create new accepted domains:
 ```
 New-AcceptedDomain -DomainName domain.com -DomainType Authoritative -Name domain.com
 New-AcceptedDomain -DomainName tenant.mail.onmicrosoft.com -DomainType Authoritative -Name tenant.mail.onmicrosoft.com
 ```

1.  Set the default accepted domain:
 ```
 Get-AcceptedDomain -Identity 'domain.com' | Set-AcceptedDomain -MakeDefault $true
 ```

1.  View the email address policy templates:
 ```
 Get-EmailAddressPolicy | fl *template*
 ```

1.  Set the email address policy with the desired templates:
 ```
 Set-EmailAddressPolicy "Default Policy" -EnabledEmailAddressTemplates "SMTP:%g.%s@domain.com","smtp:%g.%s@tenant.mail.onmicrosoft.com"
 ```

1.  Launch PowerShell with the Recipient Management snap-in:
 ```
 C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -noexit -command "Add-PSSnapin *RecipientManagement"
 ```
