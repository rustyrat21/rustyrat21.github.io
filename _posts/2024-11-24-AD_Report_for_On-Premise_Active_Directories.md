---
title: AD Report for On-Premise Active Directories
date: 2024-11-24 23:00:00 +0500
categories: [PS Modules, PowerShell, Reports]
tags: [powershell,active_directory]
---

This report will pull down users within AD and build a report around the data, output is to both a csv and excel file.

```powershell
#// Start of script
#// Get year and month for csv export file
$date = Get-Date -f "MM-dd-yy"


$SaveFolder = "Folder in which to save file to"

If ((Test-Path -path $SaveFolder) -eq $false)
{
    New-item -path $SaveFolder -ItemType "directory"
}

#// Set File format names
$CSVFile = $SaveFolder + "On-Premise_AD_Groups_" + $date + ".csv"
$ExcelFile = $SaveFolder + "On-Premise_AD_Report_" + $date + ".xlsx"

#// Create my array for CSV data
$CSVOutput = @()

#// Get all AD groups in the domain
$ADGroups = Get-ADGroup -Filter *

#// Set progress bar variables
$i = 0
$tot = $ADGroups.count

foreach ($ADGroup in $ADGroups)
{
    #// Set up progress bar
    $i++
    $status = "{0:N0}" -f ($i / $tot * 100)
    Write-Progress -Activity "Exporting AD Groups" -status "Processing Group $i of $tot : $status% Completed" -PercentComplete ($i / $tot * 100)

    #// Ensure Members variable is empty
    $Members = ""

    #// Get group members which are also groups and add to string
    $MembersArr = Get-ADGroup -filter { Name -eq $ADGroup.Name } | Get-ADGroupMember | select-object Name
    if ($MembersArr)
    {
        foreach ($Member in $MembersArr)
        {
            $Members = $Members + "," + $Member.Name
        }
        $Members = $Members.Substring(1, ($Members.Length) - 1)
    }

    #// Set up hash table and add values
    $HashTab = $NULL
    $HashTab = [ordered]@{
        "Name"     = $ADGroup.Name
        "Category" = $ADGroup.GroupCategory
        "Scope"    = $ADGroup.GroupScope
        #"Decription"	= $Desc
        "Members"  = $Members
    }

    #// Add hash table to CSV data array
    $CSVOutput += New-Object PSObject -Property $HashTab
}

#// Export to CSV files
$CSVOutput | Sort-Object Name | Export-csv -path $CSVFile -NoTypeInformation

#// Export to Excel
$CSVOutput | Sort-Object Name | Export-Excel -path $ExcelFile -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "AD Groups"
#// End of AD_Groups_Export script

#// Start of AD-Check
$AD_Check = $SaveFolder + "AD-Check_$date.xlsx"
$AD_Check_CSV = $SaveFolder + "AD-Check_$date.csv"

$ADreport = Get-ADUser -Filter * -Properties *

$ADreport | Select-Object Name, Title, Department, Manager, physicalDeliveryOfficeName, mobile, mobilenumber, StreetAddress, City, l, State, st, PostalCode, Country | Export-Excel -path $AD_Check -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet -WorksheetName "AD Users"
$ADreport | Select-Object Name, Title, Department, Manager, physicalDeliveryOfficeName, mobile, mobilenumber, StreetAddress, City, l, State, st, PostalCode, Country | Export-CSV -path $AD_Check_CSV -NoTypeInformation
#// Run compare


#//End of AD-Check

If ((Test-Path -path $SaveFolder) -eq $false)
{
    New-item -path $SaveFolder -ItemType "directory"
}
$AD_Users_CSVFile = $SaveFolder + "On-Premise_AD_Users_" + $date + ".csv"
#$on_Prem = $SaveFolder + "Users and Groups - $date.xlsx"

$Gotusers = get-aduser -Filter * -SearchBase "OU=Employees,OU=BOTJ,DC=AD,DC=BANKOFTHEJAMES,DC=BANK" -Properties "City", "Company", "Department", "Fax", "GivenName", "SurName", "sn", "Mobile", "Name", "Office", "OfficePhone", "PostalCode", "State", "StreetAddress", "telephoneNumber", "Title"

$Gotusers | Select-Object  "Name", "GivenName", "SurName", "StreetAddress", "City", "State", "PostalCode", "telephoneNumber", "OfficePhone", "Mobile", "Fax", "Title", "Department", "Office", "Company" |
Export-excel -Path $ExcelFile -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet  -WorksheetName "Users"

$Gotusers | Select-Object  "Name", "GivenName", "SurName", "StreetAddress", "City", "State", "PostalCode", "telephoneNumber", "OfficePhone", "Mobile", "Fax", "Title", "Department", "Office", "Company" |
Export-csv -Path $AD_Users_CSVFile -NoTypeInformation

#// Set CSV file name
$AD_GM_CSVFile = $SaveFolder + "On-Premise_AD_Group_Members_" + $date + ".csv"

#// Create my array for CSV data
$CSVOutput = @()

#// Get all AD groups in the domain
$ADGroups = Get-ADGroup -Filter *

#// Set progress bar variables
$i = 0
$tot = $ADGroups.count

foreach ($ADGroup in $ADGroups)
{
    #// Set up progress bar
    $i++
    $status = "{0:N0}" -f ($i / $tot * 100)
    Write-Progress -Activity "Exporting AD Groups" -status "Processing Group $i of $tot : $status% Completed" -PercentComplete ($i / $tot * 100)


    #// Ensure Members variable is empty
    $Members = ""

    #// Get group members which are also groups and add to string
    $MembersArr = Get-ADGroup -filter { Name -eq $ADGroup.Name } | Get-ADGroupMember | select-object Name
    if ($MembersArr)
    {
        foreach ($Member in $MembersArr)
        {
            $Members = $Members + "," + $Member.Name
        }
        $Members = $Members.Substring(1, ($Members.Length) - 1)
    }

    #// Set up hash table and add values
    $HashTab = $NULL
    $HashTab = [ordered]@{
        "Name"     = $ADGroup.Name
        "Category" = $ADGroup.GroupCategory
        "Scope"    = $ADGroup.GroupScope
        #"Decription"	= $Desc
        "Members"  = $Members
    }

    #// Add hash table to CSV data array
    $CSVOutput += New-Object PSObject -Property $HashTab
}

#// Export to CSV files
$CSVOutput | Export-excel -Path $ExcelFile -AutoSize -AutoFilter -FreezeTopRow -BoldTopRow -ClearSheet  -WorksheetName "Groups"
$CSVOutput | Export-Csv -path $AD_GM_CSVFile -NoTypeInformation

#// End of script

```