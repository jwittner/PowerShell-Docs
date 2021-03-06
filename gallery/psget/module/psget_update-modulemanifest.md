---
ms.date:  06/12/2017
contributor:  manikb
ms.topic:  reference
keywords:  gallery,powershell,cmdlet,psget
title:  Update-ModuleManifest
---

# Update-ModuleManifest
Updates a module manifest file.

## Description

The Update-ModuleManifest cmdlet updates a module manifest (.psd1) file.

### Notes
	- DscResourcesToExport is only supported on the latest PowerShell version 5.0. We won’t be able to update the field if you are running on lower versions of PowerShell.

## Cmdlet syntax
```powershell
Get-Command -Name Update-ModuleManifest -Module PowerShellGet -Syntax
```

## Cmdlet online help reference

[Update-ModuleManifest](http://go.microsoft.com/fwlink/?LinkId=619311)

## Example commands

This new cmdlet is used to help update manifest file with input property values. It takes all parameters that New-ModuleManifest does.

We notice that a lot of module authors would like to specify “\*” in exported values such as FunctionsToExport, CmdletsToExport, etc. During module publishing to PowerShell Gallery, unspecified functions and commands will not be populated properly onto the Gallery. Therefore, we suggest module authors update their manifests with proper values.

If you have modules that have exported properties, Update-ModuleManifest will fill the specified manifest file with information from exported functions, cmdlets, variables etc:
```powershell
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'

#(Other properties removed here for Simplicity…)

# Functions to export from this module
FunctionsToExport = '*'
# Cmdlets to export from this module
CmdletsToExport = '*'
# Variables to export from this module
VariablesToExport = '*'
# Aliases to export from this module
AliasesToExport = '*'
}
```

After Update-ModuleManifest:
```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
Get-Content -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1"
#
# Module manifest for module 'NewManifest'
#
# Generated by: author name
#
# Generated on: 11/13/2015
#
@{
# Script module or binary module file associated with this manifest.
# RootModule = ''
# Version number of this module.
ModuleVersion = '2.5'
# ID used to uniquely identify this module
GUID = '610e5c5b-dc42-4eaa-8511-ebfb44066d5e'
# Functions to export from this module
FunctionsToExport = 'Get-FooFn Get-FooWF'
# Cmdlets to export from this module
CmdletsToExport = 'Test-PSGetTestCmdlet'
}
```

For each module, there are also metadata fields associated with it. In order to display metadata properly on PowrShell Gallery, you can use Update-ModuleManifest to populate those fields under PrivateData.

```powershell
Update-ModuleManifest -Path "C:\Temp\PSGTEST-TestPackageMetadata\2.5\PSGTEST-TestPackageMetadata.psd1" -Tags "Tag1" -LicenseUri "http://license.com" -ProjectUri "http://project.com" -IconUri "http://icon.com" -ReleaseNotes "Test module"
```

PrivateData hashtable from the manifest file template has the following properties

```powershell
# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{
	PSData = @{
		# Tags applied to this module. These help with module discovery in online galleries.
		# Tags = @()

		# A URL to the license for this module.
		# LicenseUri = ''

		# A URL to the main website for this project.
		# ProjectUri = ''

		# A URL to an icon representing this module.
		# IconUri = ''

		# ReleaseNotes of this module
		# ReleaseNotes = ''

		# External dependent modules of this module
		# ExternalModuleDependencies = ''
	} # End of PSData hashtable
} # End of PrivateData hashtable
```