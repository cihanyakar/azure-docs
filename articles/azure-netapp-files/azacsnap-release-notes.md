---
title: Release Notes for Azure Application Consistent Snapshot tool for Azure NetApp Files | Microsoft Docs
description: Provides release notes for the Azure Application Consistent Snapshot tool that you can use with Azure NetApp Files. 
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''

ms.assetid:
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/24/2023
ms.author: phjensen
---

# Release Notes for Azure Application Consistent Snapshot tool

This page lists major changes made to AzAcSnap to provide new functionality or resolve defects.

Download the [latest release](https://aka.ms/azacsnapinstaller) of the installer and review how to [get started](azacsnap-get-started.md).  

For specific information on Preview features, refer to the [AzAcSnap Preview](azacsnap-preview.md) page.

## Feb-2023

### AzAcSnap 7a (Build: 1AA8343)

AzAcSnap 7a is being released with the following fixes:

- Fixes for `-c restore` commands:
  - Enable mounting volumes on HLI (BareMetal) where the volumes have been reverted to a prior state when using `-c restore --restore revertvolume`.
  - Correctly set ThroughputMiBps on volume clones for Azure NetApp Files volumes in an Auto QoS Capacity Pool when using `-c restore --restore snaptovol`.

## Dec-2022

### AzAcSnap 7 (Build: 1A8FDFF)

AzAcSnap 7 is being released with the following fixes and improvements:

- Fixes and Improvements:
  - Backup (`-c backup`) changes:
    - Shorten suffix added to the snapshot name.  The previous 26 character suffix of "YYYY-MM-DDThhhhss-nnnnnnnZ" (for example, 2022-11-17T030002-7299835Z) was too long, this is replaced with an 11 character hex-decimal equivalent based on the ten-thousandths of a second since the Unix epoch to avoid naming collisions (for example, F2D212540D5).
    - Increased validation when creating snapshots to avoid failures on snapshot creation retry.
    - Time out when executing AzAcSnap mechanism to disable/enable backint (`autoDisableEnableBackint=true`) now aligns with other SAP HANA related operation timeout values.
    - Azure Backup now allows third party snapshot-based backups without impact to streaming backups (also known as 'backint'). Therefore, AzAcSnap 'backint' detection logic has been reordered to allow for future deprecation of this feature.  By default this setting is disabled (`autoDisableEnableBackint=false`). For customers who have relied on this feature to take snapshots with AzAcSnap and use Azure Backup, keeping this value as true means AzAcSnap 7 will continue to disable/enable backint.  As this setting is no longer necessary for Azure Backup, we recommend testing AzAcSnap backups with the value of `autoDisableEnableBackint=false`, and then if successful make the same change in your production deployment.
  - Restore (`-c restore`) changes:
    - Ability to create a custom suffix for Volume clones created when using `-c restore --restore snaptovol` either:
      - via the command-line with `--clonesuffix <custom suffix>`.
      - interactively when running the command without the `--force` option.
    - When doing a `--restore snaptovol` on ANF, then Volume Clone will also inherit the new 'NetworkFeatures' setting from the Source Volume.
    - Can now do a restore if there are no Data Volumes configured.  It will only do a restore of the Other Volumes using the Other Volumes latest snapshot (the `--snapshotfilter` option only applies to Data Volumes).
    - Extra logging for `-c restore` command to help with user debugging.
  - Test (`-c test`) changes:
    - Now tests managing snapshots for all otherVolume(s) and all dataVolume(s).
- Features moved to GA (generally available):
  - None.
- Features added to [Preview](azacsnap-preview.md):
  - Preliminary support for Azure NetApp Files Backup.
  - Db2 database support adding options to configure, test, and snapshot backup IBM Db2 in an application consistent manner.
 
Download the [latest release](https://aka.ms/azacsnapinstaller) of the installer and review how to [get started](azacsnap-get-started.md).  For specific information on Preview features, refer to the [AzAcSnap Preview](azacsnap-preview.md) page.

## Jul-2022

### AzAcSnap 6 (Build: 1A5F0B8)

> [!IMPORTANT]
> AzAcSnap 6 brings a new release model for AzAcSnap and includes fully supported GA features and Preview features in a single release.  
 
Since AzAcSnap v5.0 was released as GA in April 2021, there have been eight releases of AzAcSnap across two branches. Our goal with the new release model is to align with how Azure components are released.  This change allows moving features from Preview to GA (without having to move an entire branch), and introduce new Preview features (without having to create a new branch). From AzAcSnap 6, we'll have a single branch with fully supported GA features and Preview features (which are subject to Microsoft's Preview Ts&Cs). It’s important to note customers can't accidentally use Preview features, and must enable them with the `--preview` command line option.  This means the next release will be AzAcSnap 7, which could include; patches (if necessary) for GA features, current Preview features moving to GA, or new Preview features.

AzAcSnap 6 is being released with the following fixes and improvements:

- Features moved to GA (generally available):
  - Oracle Database support.
  - Backint integration to work with Azure Backup.
  - RunBefore/RunAfter command line options to execute custom shell scripts and commands before or after taking storage snapshots.
- Features in Preview:
  - Azure Key Vault to store Service Principal content.
  - Azure Managed Disk as an alternate storage back-end.
- ANF Client API Version updated to 2021-10-01.
- Change to workflow for handling Backint to re-enable backint configuration should there be a failure when putting SAP HANA in a consistent state for snapshot.

## May-2022

### AzAcSnap v5.0.3 (Build: 20220524.14204) - Patch update to v5.0.2

AzAcSnap v5.0.3 (Build: 20220524.14204) is provided as a patch update to the v5.0 branch with the following fix:

- Fix for handling delimited identifiers when querying SAP HANA.  This issue only impacted SAP HANA in HSR-HA node when there's a Secondary node configured with 'logreplay_readaccss' and has been resolved.

Download the [latest release](https://aka.ms/azacsnapinstaller) of the installer and review how to [get started](azacsnap-get-started.md).

### AzAcSnap v5.1 Preview (Build: 20220524.15550)

AzAcSnap v5.1 Preview (Build: 20220524.15550) is an updated build to extend the preview expiry date for 90 days.  This update contains the fix for handling delimited identifiers when querying SAP HANA as provided in v5.0.3.

Read about the [AzAcSnap Preview](azacsnap-preview.md).
Download the [latest release of the Preview installer](https://aka.ms/azacsnap-preview-installer).

## Mar-2022

### AzAcSnap v5.1 Preview (Build: 20220302.81795)

AzAcSnap v5.1 Preview (Build: 20220302.81795) has been released with the following new features:

- Azure Key Vault support for securely storing the Service Principal.
- A new option for `-c backup --volume`, which has the `all` parameter value.

## Feb-2022

### AzAcSnap v5.1 Preview (Build: 20220220.55340)

AzAcSnap v5.1 Preview (Build: 20220220.55340) has been released with the following fixes and improvements:

- Resolved failure in matching `--dbsid` command line option with `sid` entry in the JSON configuration file for Oracle databases when using the `-c restore` command.

### AzAcSnap v5.1 Preview (Build: 20220203.77807)

AzAcSnap v5.1 Preview (Build: 20220203.77807) has been released with the following fixes and improvements:

- Minor update to resolve STDOUT buffer limitations.  Now the list of Oracle table files put into archive-mode is sent to an external file rather than output in the main AzAcSnap log file.  The external file is in the same location and basename as the log file, but with a ".protected-tables" extension (output filename detailed in the AzAcSnap log file).  It's overwritten each time `azacsnap` runs.

## Jan-2022

### AzAcSnap v5.1 Preview (Build: 20220125.85030)

AzAcSnap v5.1 Preview (Build: 20220125.85030) has been released with the following new features:

- Oracle Database support
- Backint Co-existence
- Azure Managed Disk
- RunBefore and RunAfter capability

## Aug-2021

### AzAcSnap v5.0.2 (Build: 20210827.19086) - Patch update to v5.0.1

AzAcSnap v5.0.2 (Build: 20210827.19086) is provided as a patch update to the v5.0 branch with the following fixes and improvements:

- Ignore `ssh` 255 exit codes.  In some cases the `ssh` command, which is used to communicate with storage on Azure Large Instance, would emit an exit code of 255 when there were no errors or execution failures  (refer `man ssh` "EXIT STATUS") - then AzAcSnap would trap this exit code as a failure and abort.  With this update extra verification is done to validate correct execution, this includes parsing `ssh` STDOUT and STDERR for errors in addition to traditional exit code checks.
- Fix the installer's check for the location of the hdbuserstore.  The installer would check for the existence of an incorrect source directory for the hdbuserstore for the user running the install - this is fixed to check for `~/.hdb`.  This fix is applicable to systems (for example, Azure Large Instance) where the hdbuserstore was pre-configured for the `root` user before installing `azacsnap`.
- Installer now shows the version it will install/extract (if the installer is run without any arguments).

## May-2021

### AzAcSnap v5.0.1 (Build: 20210524.14837) - Patch update to v5.0

AzAcSnap v5.0.1 (Build: 20210524.14837) is provided as a patch update to the v5.0 branch with the following fixes and improvements:

- Improved exit code handling.  In some cases AzAcSnap would emit an exit code of 0 (zero), even after an execution failure when the exit code should have been non-zero.  Exit codes should now only be zero on successfully running `azacsnap` to completion and non-zero if there's any failure.  
- AzAcSnap's internal error handling has been extended to capture and emit the exit code of the external commands run by AzAcSnap.

## April-2021

### AzAcSnap v5.0 (Build: 20210421.6349) - GA Released (21-April-2021)

AzAcSnap v5.0 (Build: 20210421.6349) has been made Generally Available and for this build had the following fixes and improvements:

- The hdbsql retry timeout (to wait for a response from SAP HANA) is automatically set to half of the "savePointAbortWaitSeconds" to avoid race conditions.  The setting for "savePointAbortWaitSeconds" can be modified directly in the JSON configuration file and must be a minimum of 600 seconds.

## March-2021

### AzAcSnap v5.0 Preview (Build: 20210318.30771)

AzAcSnap v5.0 Preview (Build: 20210318.30771) has been released with the following fixes and improvements:

- Removed the need to add the AZACSNAP user into the SAP HANA Tenant DBs, see the [Enable communication with database](azacsnap-installation.md#enable-communication-with-database) section.
- Fix to allow a [restore](azacsnap-cmd-ref-restore.md) with volumes configured with Manual QOS.
- Added mutex control to throttle SSH connections for Azure Large Instance.
- Fix installer for handling path names with spaces and other related issues.
- In preparation for supporting other database servers, changed the optional parameter '--hanasid' to '--dbsid'.

## Next steps

- [Get started with Azure Application Consistent Snapshot tool](azacsnap-get-started.md)
- [Download the latest release of the installer](https://aka.ms/azacsnapinstaller)
