---
title: Azure AD Weekly Report
date: 2024-11-24 23:00:00 +0500
categories: [PS Modules, PowerShell, Reports,azure]
tags: [powershell,active_directory,azure]
---

This report will connect to Azure and pull down all users from Azure AD.

```powershell
#Install-Module -Name Az -AllowClobber -Scope CurrentUser -force
#Install-module -Name AzureAD -AllowClobber -Scope CurrentUser -force
#Install-module -name Import-Excel -Scope CurrentUser -Force

Connect-AzureAD
#"Folder Location\Azuread_$date\"

$date = Get-Date -format MM-dd-yy
$SaveFolder = "Save Folder Location"



If ((Test-Path -path $SaveFolder) -eq $false)
{
    New-item -path $SaveFolder -ItemType "directory"
}
# Change over to using Export Excel and Select-Object
$FileLocation = $SaveFolder + "AzureADGroupMembership-$date.csv"
New-Item -Path $FileLocation -ItemType "File"
$Header = "Group Name" + "," + "Member Name" + "," + "Object Type" + "," + "User Type" + "," + "UPN" + "," + "Object ID"
Add-Content -Path $FileLocation -Value $Header
# Change over to using Export Excel and Select-Object
$GroupFileLocation = $SaveFolder + "AzureADGroup-$date.csv"
New-Item -Path $GroupFileLocation -ItemType "File"
#$GroupFileLocation = $SaveFolder + "AzureADGroup_$date.xlsx"
#$Header = "Group Name" + "," + "Object ID" + "," + "Mail" + "," + "Mail Enabled" + "," + "Security Enabled" + "," + "Owner" + "," + "Description"
Add-Content -Path $GroupFileLocation -Value $Header

$groups = Get-AzureADGroup -All $true

$azurefilepath = $SaveFolder + "AzureADGroup-$date.xlsx"


#Outputs the Groups and group data
Foreach ($group in $groups)
{
    $DisplayName = $group.DisplayName
    $ObjectId = $group.ObjectId
    $GMail = $group.Mail
    $GmailEnabled = $group.MailEnabled
    $GSecurity = $group.SecurityEnabled
    $Gowner = (Get-AzureADGroupOwner -ObjectId "$ObjectId").DisplayName
    $GDesc = $group.Description
    #PS-Custom Object to export to Excel
    $groupList = $DisplayName + "," + $ObjectId + "," + $GMail + "," + $GmailEnabled + "," + $GSecurity + "," + $GOwner + "," + $GDesc
    Add-Content -Path $GroupFileLocation -Value $groupList 
}

#Outputs the groups with its members data
ForEach ($group in $groups)
{
    $members = Get-AzureADGroupMember -ObjectId $group.ObjectId -All $true 
    ForEach ($member in $members)
    {
        $GroupName = $group.DisplayName
        $MemName = $member.DisplayName
        $objType = $member.ObjectType
        $userType = $member.UserType
        $UPN = $member.UserPrincipalName
        $memObjectID = $member.ObjectId
        
        $groupmemList = $GroupName + "," + $MemName + "," + $objType + "," + $userType + "," + $UPN + "," + $memObjectID
        Add-Content -Path $FileLocation -Value $groupmemList 
    }
}

Import-csv -path $GroupFileLocation | Export-Excel -path $azurefilepath -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Azure AD Groups"
Import-csv -Path $FileLocation | Export-Excel -path $azurefilepath -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Azure AD Groups Membership"

$location = "Save Folder Location"

$winT = Get-AzureADDevice -Filter "startswith(DisplayName,'?')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant
$winS = Get-AzureADDevice -Filter "startswith(DisplayName,'?')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant
$winM = Get-AzureADDevice -Filter "startswith(DisplayName,'?')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant

$OverallWindows = $winT + $winS + $winM

$OverallWindows | Export-Excel -Path $location -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Windows"

$android = Get-AzureADDevice -All 0 -Filter "startswith(DeviceOSType,'Android')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant

$apple = Get-AzureADDevice -All 0 -Filter "startswith(DeviceOSType,'iPhone')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant
$apple += Get-AzureADDevice -All 0 -Filter "startswith(DeviceOSType,'iPad')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant
$apple += Get-AzureADDevice -All 0 -Filter "startswith(DeviceOSType,'iOS')" | Select-Object -Property DisplayName, DeviceOSType, ApproximateLastLogonTimeStamp, DeviceID , ObjectID, IsCompliant

# $win | Export-Excel -Path $location -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Windows"

$android | Export-Excel -Path $location -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Android"
$apple | Export-Excel -Path $location -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "Apple"

Disconnect-AzureAD

#Get-AzureADDevice -Filter "startswith(DeviceOSType,'Windows')" | Select-Object -Property * | Out-GridView
```
