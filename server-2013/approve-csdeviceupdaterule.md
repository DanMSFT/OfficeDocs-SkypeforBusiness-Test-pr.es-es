﻿---
title: Approve-CsDeviceUpdateRule
TOCTitle: Approve-CsDeviceUpdateRule
ms:assetid: d82e0494-4868-4d13-ac03-e5a5d0f2380e
ms:mtpsurl: https://technet.microsoft.com/es-es/library/Gg398949(v=OCS.15)
ms:contentKeyID: 48276842
ms.date: 01/07/2017
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Approve-CsDeviceUpdateRule

 

_**Última modificación del tema:** 2015-03-09_

Approves a device update rule that has been imported to the system. After a device update rule has been approved, the corresponding update will automatically be downloaded and installed by client devices affected by the update. Este cmdlet se presentó en Lync Server 2010.

## Sintaxis

    Approve-CsDeviceUpdateRule [-Identity <XdsIdentity>] <COMMON PARAMETERS>

    Approve-CsDeviceUpdateRule [-Instance <PSObject>] <COMMON PARAMETERS>

    COMMON PARAMETERS: [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

The command shown in Example 1 approves the device update rule d5ce3c10-2588-420a-82ac-dc2d9b1222ff9 found on the service WebServer:atl-cs-001.litwareinc.com.

    Approve-CsDeviceUpdateRule -Identity service:WebServer:atl-cs-001.litwareinc.com/d5ce3c10-2588-420a-82ac-dc2d9b1222ff9

## EXAMPLE 2

Example 2 approves all the device update rules that have been configured for the service WebServer:atl-cs-001.litwareinc.com. To do this, the command first calls the **Get-CsDeviceUpdateRule** cmdlet along with the Filter parameter; the filter value "service:WebServer:atl-cs-001.litwareinc.com\*" ensures that only those rules that have an Identity that begins with the string value "service:WebServer:atl-cs-001.litwareinc.com" will be returned. (By definition, these are all the device update rules that have been assigned to the service WebServer:atl-cs-001.litwareinc.com.) This filtered collection is then piped to the **Approve-CsDeviceUpdateRule** cmdlet, which approves each rule in the collection.

    Get-CsDeviceUpdateRule -Filter service:WebServer:atl-cs-001.litwareinc.com* | Approve-CsDeviceUpdateRule

## EXAMPLE 3

The command shown in Example 3 approves all the device update rules for the specified brand (LG-Nortel). To do this, the command first calls the **Get-CsDeviceUpdateRule** cmdlet to return a collection of all the device update rules currently in use in the organization. This collection is then piped to the **Where-Object** cmdlet, which picks out only those rules where the Brand property is equal to LG-Nortel. The filtered collection is then piped to the **Approve-CsDeviceUpdateRule** cmdlet, which approves each rule in the collection.

    Get-CsDeviceUpdateRule | Where-Object {$_.Brand -eq "LG-Nortel"} | Approve-CsDeviceUpdateRule

## Detailed Description

Lync Server uses device update rules as a way to provide firmware updates to devices that run Lync Phone Edition. Periodically, administrators upload a set of device update rules to Lync Server; after those rules have been tested and approved, they are automatically downloaded and applied to the appropriate devices as those devices connect to the system. By default, devices check for new update rules each time they turn on and connect to Lync Server. Devices also check for updates every 24 hours after that initial sign on.

Each new device update rule added to the system is marked as "Pending." That means that the update will be downloaded and installed by the appropriate test devices; however, it will not be downloaded and installed by client devices in general. This gives you an opportunity to test the updates and ensure that there are no adverse effects before you make this update widely available. As soon as you are convinced that the update has passed your tests and will work for your organization, you can then use the **Approve-CsDeviceUpdateRule** cmdlet to approve the update.

When you approve an update, the PendingVersion of the associated update rule is assigned to the ApprovedVersion, and the PendingVersion property is cleared. For example, suppose the PendingVersion of a new update rule is version 1.0.0.1. After you run the **Approve-CsDeviceUpdateRule** cmdlet, the PendingVersion will be set to a null value, and the ApprovedVersion will be set to 1.0.0.1. The next time a client device logs on, the device will automatically check to see if there are any newly-approved updates applicable for that device. If so, the update will automatically be downloaded and installed.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Approve-CsDeviceUpdateRule** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Approve-CsDeviceUpdateRule"}

## Parámetros


<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Required</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Se le pedirá confirmación antes de ejecutar el comando.</p></td>
</tr>
<tr class="even">
<td><p><em>Force</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Suppresses the display of any non-fatal error message that might occur when running the command.</p></td>
</tr>
<tr class="odd">
<td><p><em>Identity</em></p></td>
<td><p>Optional</p></td>
<td><p>Microsoft.Rtc.Management.Xds.XdsIdentity</p></td>
<td><p>Unique identifier for the device update rule being approved. The Identity for a device update rule consists of two parts: the service where the device update rule has been assigned (for example, service:WebServer:atl-cs-001.litwareinc.com) and a globally unique identifier (GUID). Consequently, a device update rule configured for the Redmond site will have an Identity similar to this: service:WebServer:atl-cs-001.litwareinc.com /d5ce3c10-2588-420a-82ac-dc2d9b1222ff9.</p></td>
</tr>
<tr class="even">
<td><p><em>Instance</em></p></td>
<td><p>Optional</p></td>
<td><p>DeviceUpdate.Rule object</p></td>
<td><p>Permite transmitir una referencia a un objeto en el cmdlet en lugar de establecer valores de parámetro independientes.</p></td>
</tr>
<tr class="odd">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Describe qué sucedería si se ejecutara el comando sin ejecutarlo realmente.</p></td>
</tr>
</tbody>
</table>


## Input Types

Microsoft.Rtc.Management.WritableConfig.Settings.DeviceUpdate.DeviceUpdate.Rule object. The **Approve-CsDeviceUpdateRule** cmdlet accepts pipelined instances of the device update rule object.

## Return Types

None. Instead, the **Approve-CsDeviceUpdateRule** cmdlet approves instances of the Microsoft.Rtc.Management.WritableConfig.Settings.DeviceUpdate.DeviceUpdate.Rule object.

## Vea también

#### Otros recursos

[Get-CsDeviceUpdateRule](get-csdeviceupdaterule.md)  
[Remove-CsDeviceUpdateRule](remove-csdeviceupdaterule.md)  
[Reset-CsDeviceUpdateRule](reset-csdeviceupdaterule.md)  
[Restore-CsDeviceUpdateRule](restore-csdeviceupdaterule.md)

