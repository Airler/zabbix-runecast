# Zabbix template for Runecast monitoring

## Overview

The template to monitor Runecast by Zabbix 6.x using HTTP agent.

## Setup

> See [Zabbix templates importing](https://www.zabbix.com/documentation/6.0/manual/xml_export_import/templates#importing) for basic instructions on how to import a template.

1. Create a new host
2. Set/change the host macros required for Runecast API authentication:
```text
{$RUNECAST_API_TOKEN}
```
3. Link the template to host created early

## Zabbix configuration

No specific Zabbix configuration is required

### Macros used

|Name|Description|Default|
|----|-----------|-------|
|{$RUNECAST_API_TOKEN} |<p>Runecast API token. (go to Runecast server > Settings > API Access Token to generate one; token only needs read permissions)</p> | |

## Template links

There are no template links in this template.

## Discovery rules

|Name|Description|Type|Key and additional info|
|----|-----------|----|----|
|Analysis discovery|Discovery of analysis details, e.g. status, timestamp, issues|DEPENDENT|runecast.analysis.discovery <p>**Preprocessing**:<br>- DISCARD_UNCHANGED_HEARTBEAT: `6h`</p> |
|License detail discovery|Discovery of license details|DEPENDENT|runecast.license.discovery <p>**Preprocessing**:<br>- JSONPath: `$.licenses`<br>- DISCARD_UNCHANGED_HEARTBEAT: `6h`</p> |
|vCenter discovery|Discovery of connected vCenters|DEPENDENT|runecast.vcenter.discovery <p>**Preprocessing**:<br>- DISCARD_UNCHANGED_HEARTBEAT: `6h`</p> |

## Items collected

|Name|Description|Type|Key and additional info|
|----------|--------------|----------|----------|
|Runecast: Analysis results| |HTTP_Agent|runecast.analysis.results <p>**Preprocessing**:<br>- JSONPath: `$.results`</p> |
|Runecast: License info| |HTTP_Agent|runecast.license.info |
|Runecast: licensed hosts cpus| |DEPENDENT|runecast.license.info.licensed.hostcpus <p>**Preprocessing**:<br>- JSONPath: `$.licensedHostCPUs`</p> |
|Runecast: licensed hosts| |DEPENDENT|runecast.license.info.licensed.hosts <p>**Preprocessing**:<br>- JSONPath: `$.licensedHosts`</p> |
|Runecast: unlicensed hosts cpus| |DEPENDENT|runecast.license.info.unlicensed.hostcpus <p>**Preprocessing**:<br>- JSONPath: `$.unlicensedHostCPUs`</p> |
|Runecast: unlicensed hosts| |DEPENDENT|runecast.license.info.unlicensed.hosts <p>**Preprocessing**:<br>- JSONPath: `$.unlicensedHosts`</p> |
|Runecast: vCenter info| |HTTP_Agent|runecast.vcenter.info <p>**Preprocessing**:<br>- JSONPath: `$.vcenters`</p> |
|Runecast: Analysis issues| |DEPENDENT|runecast.analysis.issues["{#ANALYSIS_UID}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.uid== "{#ANALYSIS_UID}")].issues.first()`<br>- DISCARD_UNCHANGED_HEARTBEAT: `6h`</p> |
|Runecast: Analysis scan status| |DEPENDENT|runecast.analysis.scanstatus["{#ANALYSIS_UID}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.uid== "{#ANALYSIS_UID}")].scanStatus.status.first()`</p> |
|Runecast: last analysis scan| |DEPENDENT|runecast.analysis.scantimestamp["{#ANALYSIS_UID}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.uid== "{#ANALYSIS_UID}")].scanStatus.timestamp.first()`</p> |
|Runecast: License allowed CPUs| |DEPENDENT|runecast.license.detail.allowedcpus["{#LICENSE_ID}"] <p>**Preprocessing**:<br>- JSONPath: `$.licenses.[?(@.id== "{#LICENSE_ID}")].allowedCPUs.first()`<br>- DISCARD_UNCHANGED_HEARTBEAT: `6h`</p> |
|Runecast: License used CPUs| |DEPENDENT|runecast.license.detail.usedcpus["{#LICENSE_ID}"] <p>**Preprocessing**:<br>- JSONPath: `$.licenses.[?(@.id== "{#LICENSE_ID}")].usedCPUs.first()`</p> |
|Runecast: License valid until| |DEPENDENT|runecast.license.detail.validuntil["{#LICENSE_ID}"] <p>**Preprocessing**:<br>- JSONPath: `$.licenses.[?(@.id== "{#LICENSE_ID}")].validUntil.first()`</p> |
|Runecast: {#VCENTER_ADDR}: plugin installed| |DEPENDENT|runecast.vcenter.rcplugin.installed["{#VCENTER_ADDR}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.address == "{#VCENTER_ADDR}")].webClientPluginStatus.installed.first()`</p> |
|Runecast: {#VCENTER_ADDR}: plugin latest| |DEPENDENT|runecast.vcenter.rcplugin.latest["{#VCENTER_ADDR}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.address == "{#VCENTER_ADDR}")].webClientPluginStatus.latest.first()`</p> |
|Runecast: {#VCENTER_ADDR}: plugin version| |DEPENDENT|runecast.vcenter.rcplugin.version["{#VCENTER_ADDR}"] <p>**Preprocessing**:<br>- JSONPath: `$.[?(@.address == "{#VCENTER_ADDR}")].webClientPluginStatus.version.first()`</p> |

## Triggers

Appropriate triggers are associated with the items

## Feedback

Please report any issues with the template here in the "Issues" tab.

## ToDo
1. monitor Issue events/details
