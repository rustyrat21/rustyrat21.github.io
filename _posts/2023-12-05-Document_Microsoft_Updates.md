---
title: Documentation on several Microsoft OS, Platforms, and Applications
date: 2023-12-5 23:00:00 +0500
categories: [PS Modules, PowerShell, Microsoft Updates]
tags: [powershell,get-mscatalogupdate,microsoft,updates]
---


This article will demostrate how to use the PowerShell (PS) module Get-MSCatalogUpdate to pull down data for Microsoft patches, for a variety of Microsoft OSes, Platforms, and Applications.

This module can be found under ryan-jan's github account at https://github.com/ryan-jan/MSCatalog.  

## Get-MSCatalogUpdate
Get-MSCatalogUpdate pulls data from https://www.catalog.update.microsoft.com/Home.aspx based on given parameters.

### Before you write your script
- Make sure you already know what you want to download, be specific as this site houses A LOT of data.
- Know where you want to store the files that you are going to generate.
- Make sure that using PowerShell this way is authorized in your environment.

## Code
```powershell
$date = Get-date -Format "MM-dd-yy"
$location = "\\Full UNC file path\$date"

$1809 = "Win10_1809_updates.csv"
$1903 = "Win10_1903_updates.csv"
$1909 = "Win10_1909_updates.csv"
$2004 = "Win10_2004_updates.csv"
$2009 = "Win10_2009_updates.csv"
$20H2 = "Win10_20H2_updates.csv"
$21H1 = "Win10_21H1_updates.csv"
$21H2 = "Win10_21H2_updates.csv"
$21H1 = "Win10_22H1_updates.csv"
$21H2 = "Win10_22H2_updates.csv"
$Win11 = "Win11_Updates.csv"
$Win12r2 = "Win12r2_updates.csv"
$Win16 = "Win16_updates.csv"
$win19 = "Win19_updates.csv"
$win22 = "Win22_updates.csv"
$MS_Intel = "intel_updates.csv"
$Win10 = "Win10_updates.csv"
$SQL14 = "SQL14_updates.csv"
$SQL16 = "SQL16_updates.csv"
$SQL17 = "SQL17_updates.csv"
$SQL19 = "SQL19_updates.csv"
$SQL22 = "SQL22_updates.csv"
$SSMS = "SSMS_updates.csv"
$Office = "Office_updates.csv"
$Visio = "Office_updates.csv"
$Project = "Office_updates.csv"

# This test will check to verify if the new folder has laready been generated, if it has not then it will be generated using $date as the folder name

$test = Test-Path -Path $location
if($test -eq $false){
New-Item -Path $location -ItemType "Directory"
}

# Workstation Updates

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 1809" | Export-csv -path "$location\$1809" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (1809)" | Export-csv -path "$location\$1809" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 1903" | Export-csv -path "$location\$1903" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (1903)" | Export-csv -path "$location\$1903" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 1909" | Export-csv -path "$location\$1909" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (1909)" | Export-csv -path "$location\$1909" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 2004" | Export-csv -path "$location\$2004" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (2004)" | Export-csv -path "$location\$2004" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 2009" | Export-csv -path "$location\$2009" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (2009)" | Export-csv -path "$location\$2009" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 20H2" | Export-csv -path "$location\$20H2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (20H2)" | Export-csv -path "$location\$20H2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 21H1" | Export-csv -path "$location\$21H1" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (21H1)" | Export-csv -path "$location\$21H1" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 10 Version 21H2" | Export-csv -path "$location\$21H2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 10 (21H2)" | Export-csv -path "$location\$21H2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Windows 10" | Export-Csv -path "$location\$Win10" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows 11" | Export-csv -path "$location\$Win11" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows 11" | Export-csv -path "$location\$Win11" -NoTypeInformation -Append

# Server Updates

Get-MSCatalogUpdate -Search "Cumulative Update for Windows Server 2012 R2" | Export-csv -path "$location\$Win12r2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Server 2012 R2" | Export-csv -path "$location\$Win12r2" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows Server 2016" | Export-csv -path "$location\$Win16" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Server 2016" | Export-csv -path "$location\$Win16" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows Server 2019" | Export-csv -path "$location\$Win19" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows Server 2019" | Export-csv -path "$location\$Win19" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Cumulative Update for Windows Server 2022" | Export-csv -path "$location\$Win22" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for Windows Server 2022" | Export-csv -path "$location\$Win22" -NoTypeInformation -Append


# MS App Updates
Get-MSCatalogUpdate -Search "Microsoft Defender Antivirus" | Export-Csv -path "$location\$MS_Intel" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Intelligence Update" | Export-Csv -path "$location\$MS_Intel" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Microsoft Office" | Export-Csv -path "$location\$Office" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Microsoft Visio" | Export-Csv -path "$location\$Visio" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Microsoft Project" | Export-Csv -path "$location\$Project" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "SQL Server Management Studio" | Export-Csv -path "$location\$SSMS" -NoTypeInformation -Append

# SQL Server Updates

Get-MSCatalogUpdate -Search "SQL Server 2014 TRM Cumulative Update" | Export-csv -path "$location\$SQL14" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for SQL Server 2014" | Export-csv -path "$location\$SQL14" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "SQL Server 2016 TRM Cumulative Update" | Export-csv -path "$location\$SQL16" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for SQL Server 2016" | Export-csv -path "$location\$SQL16" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "SQL Server 2017 TRM Cumulative Update" | Export-csv -path "$location\$SQL17" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for SQL Server 2017" | Export-csv -path "$location\$SQL17" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "SQL Server 2019 TRM Cumulative Update" | Export-csv -path "$location\$SQL19" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for SQL Server 2019" | Export-csv -path "$location\$SQL19" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "SQL Server 2022 TRM Cumulative Update" | Export-csv -path "$location\$SQL22" -NoTypeInformation -Append

Get-MSCatalogUpdate -Search "Security Update for SQL Server 2022" | Export-csv -path "$location\$SQL22" -NoTypeInformation -Append
```
