# Autoaci
Autoaci is an automation tool that performs the following tasks of the Cisco ACI configuration.

1. Policy Model Configuration (EPG and Bridge Domain)
2. Port Channel Configuration
3. VPC Configuration
4. Orphan Interface Configuration
5. EPG to Port Channel Interface Mapping
6. EPG to VPC Mapping
7. EPG to Orphan Interface Mapping
8. Local APIC Username
9. Local APIC Username SSH Key
10. Fast LACP Timer
11. OOB Contract
12. Ingress Based ACL's
13. Export Contracts
14. Taboo Contract
15. Contract with Optimized TCAM usage
16. Intra EPG Contract

It is expected that the end user is a network SME and familiar with Cisco ACI. The input data is
provided in an Excel spreadsheet.

## System Components
![System Components](pics/image001.png)

### autoaci.py
autoaci.py is a Python Script which reads and pre-processes configuration parameters from the input
spreadsheet and executes the Ansible playbook required for the given scenario.

### Ansible
Ansible is an IT automation platform which includes many modules compatible with most
vendors. In this project Ansible is used to configure APIC with REST API calls. The
configuration steps to be performed on the target system are defined in an Ansible playbook.

### Tasks
While it is possible to write all code in a single playbook, for a complex scenario it is better to
split the objectives into blocks, each representing a simple action like the creation or deletion of
a single object, and to put these in a separate file. This approach allows the same tasks to be
imported into other playbooks and reused in various automation scenarios.
Below is the full list of tasks. All use the aci_rest module to perform a REST API call in order to
manage ACI objects.

Task | Task File 
---- | --------- 
Add Access Policy Group | playbooks/aci/atomic/access_policy_group/add/task.yml 
Delete Access Policy Group | playbooks/aci/atomic/access_policy_group/delete/task.yml
Add Bridge Domain | playbooks/aci/atomic/bridge_domain/add/task.yml
Delete Bridge Domain | playbooks/aci/atomic/bridge_domain/delete/task.yml
Add EPG | playbooks/aci/atomic/epg/add/task.yml
Delete EPG | playbooks/aci/atomic/epg/delete/task.yml
Add EPG to Interface | Mapping playbooks/aci/atomic/epg_to_interface/add/task.yml
Delete EPG to Interface Mapping | playbooks/aci/atomic/epg_to_interface/delete/task.yml
Add EPG to PC Mapping | playbooks/aci/atomic/epg_to_pc/add/task.yml
Delete EPG to PC Mapping | playbooks/aci/atomic/epg_to_pc/delete/task.yml
Add EPG to Physical Domain Mapping | playbooks/aci/atomic/epg_to_physical_domain/add/task.yml
Add EPG to VPC Mapping | playbooks/aci/atomic/epg_to_vpc/add/task.yml
Delete EPG to VPC Mapping | playbooks/aci/atomic/epg_to_vpc/delete/task.yml
Add Interface Profile | playbooks/aci/atomic/interface_profile/add/task.yml
Delete Interface Profile | playbooks/aci/atomic/interface_profile/delete/task.yml
Add Interface Selector | playbooks/aci/atomic/interface_selector/add/task.yml
Add Interface Selector to Access Policy Group Mapping | playbooks/aci/atomic/interface_selector_to_acces_policy_group/add/task.yml
Add Interface Selector to PC Policy Group Mapping | playbooks/aci/atomic/interface_selector_to_pc_policy_group/add/task.yml
Add Interface Selector to VPC Policy Group Mapping | playbooks/aci/atomic/interface_selector_to_vpc_policy_group/add/task.yml
If VLAN in Physical | Domain playbooks/aci/atomic/is_vlan_in_phisycal_domain/query/task.yml
Add Leaf Profile | playbooks/aci/atomic/leaf_profile/add/task.yml
Add PC Policy Group | playbooks/aci/atomic/pc_policy_group/add/task.yml
Delete PC Policy Group | playbooks/aci/atomic/pc_policy_group/delete/task.yml
Add Port Block | playbooks/aci/atomic/port_block/add/task.yml
Add Subnet | playbooks/aci/atomic/subnet/add/task.yml
Add VPC Policy Group | playbooks/aci/atomic/vpc_policy_group/add/task.yml
Delete VPC Policy Group | playbooks/aci/atomic/vpc_policy_group/delete/task.yml
Add Local APIC Username | playbooks/aci/tasks/local_apic_username/add/task.yml
Delete Local APIC Username | playbooks/aci/tasks/local_apic_username/delete/task.yml
Reset Local APIC User Password | playbooks/aci/tasks/local_apic_username/reset/task.yml
Add Local APIC Username SSH Key | playbooks/aci/tasks/local_apic_username_ssh_key/add/task.yml
Delete Local APIC Username SSH Key | playbooks/aci/tasks/local_apic_username_ssh_key/delete/task.yml
Add Fast LACP Timer Policy | playbooks/aci/tasks/fast_lacp_timer_policy/add/task.yml
Add Fast LACP Timer Override Policy | playbooks/aci/tasks/fast_lacp_timer_override_policy/add/task.yml
Add Fast LACP Timer To vPC | playbooks/aci/tasks/fast_lacp_timer_to_vpc/add/task.yml
Create Switch OOB Contract | playbooks/aci/tasks/switch_oob_contract/add/task.yml
Create Default Switch OOB Contract | playbooks/aci/tasks/switch_oob_contract/default/task.yml
Consume Switch OOB Contract | playbooks/aci/tasks/switch_oob_contract_consume/add/task.yml
Consume Default Switch OOB Contract | playbooks/aci/tasks/switch_oob_contract_consume/default/task.yml
Create Switch OOB Contract Filter | playbooks/aci/tasks/switch_oob_contract_filter/add/task.yml
Create Switch OOB Contract Permit Only Source IP | playbooks/aci/tasks/switch_oob_contract_permit
Change Switch OOB IP Address | playbooks/aci/tasks/switch_oob_ip_address
Ingress Based ACL | playbooks/aci/tasks/ingress_based_acl/add/task.yml
Export Contracts | playbooks/aci/tasks/export_contracts/add/task.yml
Create Taboo Contract | playbooks/aci/tasks/taboo_contract/add/task.yml
Add Taboo Contract to EPG | playbooks/aci/tasks/taboo_contract_to_epg/add/task.yml
Filter to Subject TCAM Optimized | playbooks/aci/tasks/filter_to_subject_tcam_optimized/add/task.yml
Intra EPG Contract | playbooks/aci/tasks/contract_intra_epg/add/task.yml
Custom Filter to Subject | playbooks/aci/tasks/filter_to_subject_custom/add/task.yml


### Playbooks
A dedicated playbook was developed for each automation objective, and imports the tasks
required.

Objective | Playbook
--- | ---
Policy Model Configuration | playbooks/aci/scenarios/policy_model/playbook.yml
Port Channel Configuration | playbooks/aci/scenarios/pc/playbook.yml
VPC Configuration | playbooks/aci/scenarios/vpc/playbook.yml
Orphan Interface Configuration | playbooks/aci/scenarios/orphan/playbook.yml
EPG to Port Channel Interface Mapping | playbooks/aci/scenarios/epg_to_pc/playbook.yml
EPG to VPC Mapping | playbooks/aci/scenarios/epg_to_vpc/playbook.yml
EPG to Orphan Interface Mapping | playbooks/aci/scenarios/epg_to_orphan/playbook.yml
Run a single task | run_atomic_task/playbook.yml
Local APIC Username | playbooks/aci/playbooks/local_apic_username/playbook.yml
Fast LACP Timer | playbooks/aci/playbooks/fast_lacp_timer/playbook.yml
Switch OOB | playbooks/aci/playbooks/switch_oob/playbook.yml
Ingress Based ACL | playbooks/aci/playbooks/ingress_based_acl/playbook.yml
Export Contracts | playbooks/aci/playbooks/export_contracts/playbook.yml
Taboo Contract | playbooks/aci/playbooks/taboo_contract/playbook.yml
Contract TCAM Optimized | playbooks/aci/playbooks/contract_tcam_optimized/playbook.yml
Intra EPG Contract | playbooks/aci/playbooks/contract_intra_epg/playbook.yml

For a detailed description, see ‘Usage’.

### Action Plugins
Action plugins combine module functionality with local data processing. The development of an
action plugin makes sense for tasks that require processing of the output which cannot be
performed by the aci_rest module or by built-in Ansible filters. For this project the plugin
aci_is_vlan_in_physical_domain has been developed. It checks whether a VLAN ID belongs to
any of the VLAN ranges associated with the VLAN pools of a physical domain. This plugin is
used by the “If VLAN in Physical Domain” task.

### Playbook Directory Structure
The organisation of the folder with Ansible related scripts and configuration is shown below.
![directory structure](pics/image002.png)

The parent folder for Ansible playbook related files is “playbooks”. “aci” is the folder for all playbooks and tasks for Cisco ACI. “tasks”
is the folder that contains task-related files. Tasks are grouped in folders named after the object
they configure. For example, the “EPG” object can be added or deleted, so it has two
corresponding tasks in the following path: playbooks/atomic/epg/add/task.yml and 
playbooks/atomic/epg/delete/task.yml. “playbooks” is the folder for playbooks. “hosts” is the
inventory file, which describes targets. In this case it contains only Cisco APICs. The “action
plugins” contain plugins.

### Input spreadsheet

An input spreadsheet contains input parameters. Each scenario has its own tab in this sheet. This
sheet should be placed in the ‘input’ folder.

### APIC
APIC is the unified point of management for the Cisco ACI, which allows the configuration to
be applied using REST API calls.

## Installation
To simplify the installation, all components are delivered in a container, which should be 
deployed on a Linux workstation with Docker (or Podman). All the operations are performed within
container. Container already contains the most recent version of code (python scripts, Ansible
playbooks and tasks). Project files reside in the /usr/local/autoaci folder within container and 
in order to run playbooks one should log in to container, navigate to /usr/local/autoaci folder 
and run required scenario (i.e. Ansible playbook) with autoaci.py script. 

### Container Deployment
The commands listed below are related to Docker but in case Podman is used instead, command syntax 
is pretty the same.
- Navigate to https://ibm.box.com/s/3erfivz108trdhmfa049i5e8atncm3br box folder and pick the most recent 
  container version
- Put the container image autoaci_v<version>_container_image_<date>.tgz into the /tmp folder.
- Load the image: `docker load -i /tmp/autoaci_v<version>_container_image_<date>.tgz`.
- Run container with `docker run -dit -m 400M --name autoaci autoaci:<container version>`
- Log in to container with `docker exec -it -w /usr/local/autoaci autoaci /bin/bash`

## Usage
To perform ACI configuration, the user should fill in the input spreadsheet and execute the
autoaci.py script. A detailed description of the input parameters and a tab name for each scenario
are given below in the corresponding sections.

Input spreadsheet can be editied outside container and uploaded to the container with:
`docker cp /<path to input spreadsheet/autoaci_input.xls  autoaci:/usr/local/autoaci/input/autoaci_input.xls`

It is assumed that all commands shown in this section are executed from the /usr/local/autoaci
folder.

The command to execute the script is `python ./autoaci.py -f <input file>  -s <objective> -u <username> -p <password>`.

- s defines which scenario to execute. The possible values are:
  - policy_model - Policy Model Configuration
  - pc – Port Channel Configuration
  - vpc – VPC Configuration
  - orphan – Orphan Interface Configuration
  - epg_to_pc - EPG to Port Channel Mapping
  - epg_to_vpc - EPG to VPC Mapping
  - epg_to_port - EPG to Orphan Port Mapping
  - all – All objectives
- f defines the input file. One should put autoaci_input.xls for <input file>
- u username. Here APIC username is implied
- p password. Here APIC password is implied

## Python Libraries Installation
Even though container already includes all required Python libraries, modules it may be required to
perform library bulk update in case of some issues. This may be achieved with a single command:
pip3 install -r <path_to_the_file>/requirements.txt

This command references requirements.txt file which should be created manually and may include the list of all 
required libraries. Example:

#file begining  
chardet==3.0.4  
cryptography==2.8  
et-xmlfile==1.0.1  
idna==2.8  
jdcal==1.4.1  
Jinja2==2.10.3  
MarkupSafe==1.1.1  
numpy==1.17.4  
openpyxl==2.6.2  
paramiko==2.6.0  
pycparser==2.19  
PyNaCl==1.3.0  
python-dateutil==2.8.1  
pytz==2019.3  
PyYAML==5.2  
requests==2.21.0  
setuptools==42.0.2  
six==1.13.0  
urllib3==1.24.3  
xlrd==1.2.0  
pandas==0.24.2  
yamllint==1.26.0  
ansible-lint==4.3.7  
ansible==2.9.15  
ansible-runner==1.4.7  
#file ending  

### Policy Model Configuration
The goal of this task is the configuration of policy model objects. It creates a bridge domain (L2
or L3) and an EPG, and links the EPG to a physical domain.

#### Input Parameters
The input parameters for this scenario are defined on the tab “policy_model” and are listed
below.

Column | Description 
 --- | --- 
Tenant | Name of the existing tenant, to which the new bridge domain and the new EPG will belong.
VRF | The name of the existing VRF to which the bridge domain will belong.
Bridge Domain | The name of the bridge domain to be created.
ARP Flooding | Defines whether ARP flooding is enabled. The possible values are “yes” or “no”
L2 Unknown Unicast | Defines the handling of the Layer 2 unknown unicast traffic. Can be set to flood or hardware proxy.
Unicast Routing | If this option is defined and a subnet address is set, the fabric provides the default gateway function and routes the traffic. The possible values are “yes” or “no”.
Gateway | Defines the default gateway for the bridge domain (SVI IP address).
Scope | Defines the scope of the subnet. The possible values are “public”, “private” and “shared”.
EPG | The name of the EPG to be created.
Application Profile | The name of the existing application profile to which the EPG will belong.
Physical Domain | The name of the existing physical domain to which the EPG should be linked.
Status | Identifies whether the row should be processed or skipped. The possible values are “created” or “skipped”.

#### Workflow
![Policy Model Configuration](pics/image003.png)

### Configuration of Port Channel
To configure a port channel the system creates an Interface Policy Group, Interface Profile,
Interface Selector and a Leaf Profile. For the Interface Selector the same name is used as for the
Interface Policy Group. The name of the Interface Profile is generated automatically in the format: 
leaf\<Switch ID\>_intpfl. If some of the above objects already exist in the ACI fabric, they
will be reused for configuration.

#### Input Parameters
The input parameters for this task are defined on the tab “pc”.

Column | Description
---|---
AAEP | An attachable access entity profile to which the interface policy group should be linked.
Policy Group | The name of the Interface Policy Group to be created.
CDP Policy | The name of the CDP policy to be associated with the interface policy.
LLDP Policy | The name of the LLDP policy to be associated with the interface policy.
LACP Policy | The name of the LACP policy to be associated with the interface policy.
From Port | The first port of a Port Block of the Interface Selector.
To Port | The last port of a Port Block of the Interface Selector.
Port Block Description | The description of the port block associated with the port channel.
Switch Profile | The name of the Leaf Profile.
Switch ID | The node ID of the switch.

#### Workflow
![Port Channel Configuration](pics/image004.png)

### Configuration of VPC
To configure a VPC the system creates a policy group, interface profile, interface selector and
leaf profile. For the Interface Selector the same name is used as for the
Interface Policy Group. The name of the Interface Profile is generated automatically in the format: 
leaf\<Switch A ID\>-\<Switch B ID\>_intpfl. If some of the above objects already exist in the ACI fabric, they
will be reused for configuration.

#### Input Parameters
The input parameters for this task are defined on the tab “vpc”.

Column | Description
---|---
AAEP | An attachable access entity profile to which the interface policy group should be linked.
Policy Group | The name of the Interface Policy Group to be created.
CDP Policy | The name of the CDP policy to be associated with the interface policy.
LLDP Policy | The name of the LLDP policy to be associated with the interface policy.
LACP Policy | The name of the LACP policy to be associated with the interface policy.
From Port | The first port of a Port Block of the Interface Selector.
To Port | The last post of a Port Block of the Interface Selector.
Port Block Description | The description of the port block associated with the port channel.
Switch Profile | The name of the Leaf Profile.
Switch A ID | The node ID of the first switch in the VPC pair.
Switch B ID | The node ID of the second switch in the VPC pair.

#### Workflow
![VPC Configuration Workflow](pics/image005.png)

### Configuration of Orphan port
An orphan port or a stand-alone port is an interface which does not belong to a port channel or a
VPC. To configure an orphan port the system creates a policy group, interface profile, interface
selector and leaf profile. The name of the Interface Profile is generated automatically in the format: 
leaf\<Switch ID\>_intpfl. If some of the above objects already exist in the ACI fabric, they
will be reused for configuration.

#### Input Parameters
The input parameters for this task are defined on the tab “orphan”.

Column | Description
---|---
AAEP | An attachable access entity profile to which the interface policy group should be linked.
Policy Group | The name of the Interface Policy Group to be created.
CDP Policy | The name of the CDP policy to be associated with the interface policy.
LLDP Policy | The name of the LLDP policy to be associated with the interface policy
Port | Port number.
Port Block Description | The description of the port block associated with the port.
Switch Profile | The name of the Leaf Profile.
Switch ID | The node ID of the switch.

#### Workflow
![Orphan Port Configuration](pics/image006.png)

### EPG to Port Channel Mapping
The goal of this task is to deploy an EPG through a Policy Group to a Port Channel. Before
doing so the system checks whether the provided VLAN belongs to any VLAN pool associated
with the provided Physical Domain.

#### Input Parameters

The input parameters for this task are defined on the tab “epg_to_pc”.

Column | Description
--- | ---
Tenant | The name of the existing tenant, to which the EPG belongs.
Physical Domain | The Physical Domain which should be queried as to whether the provided VLAN belongs to any of its VLAN Pools.
Application Profile | The name of the existing application profile to which the EPG will belong.
VLAN | VLAN ID that will be used for the EPG on the interface.
Mode | Defines whether the traffic should be tagged. The possible values are “regular” for tagged and “untagged” for untagged.
Pod | Pod where the switch or switches are.
Switch ID | Node ID of the switch.
Policy Group | Interface policy group name.

#### Workflow
![PG to Port Channel Mapping](pics/image007.png)

### EPG to VPC Mapping
The goal of this task is to deploy an EPG through a Policy Group to a VPC. Before doing so the
system checks whether the provided VLAN belongs to any VLAN pool associated with the
provided Physical Domain.

#### Input Parameters
The input parameters for this task are defined on the tab “epg_to_vpc”.

Column | Description
--- | ---
Tenant | The name of the existing tenant, to which the EPG belongs.
Application Profile | The name of the existing application profile to which the EPG will belong.
Physical Domain | The Physical Domain which should be queried as to whether the provided VLAN belongs to any of its VLAN Pools.
VLAN | VLAN ID that will be used for the EPG on the interface.
Mode | Defines whether the traffic should be tagged. The possible values are “regular” for tagged and “untagged” for untagged.
Pod | Pod where the switch or switches are.
Switch A ID | The node ID of the first switch in the VPC pair.
Switch B ID | The node ID of the second switch in the VPC pair.
Policy Group | Interface policy group name.

#### Workflow
![EPG to VPC Mapping ](pics/image008.png)

### EPG to Orphan Port Mapping
The goal of this task is to deploy an EPG to an interface. Before doing so the system checks
whether the provided VLAN belongs to any VLAN pool associated with the provided Physical
Domain.

#### Input Parameters
The input parameters for this task are defined on the tab “epg_to_orphan”.

Column | Description
---|---
Tenant | The name of the existing tenant, to which the EPG belongs.
Application Profile | The name of the existing application profile to which the EPG will belong.
Physical Domain | The Physical Domain which should be queried as to whether the provided VLAN belongs to any of its VLAN Pools.
VLAN | VLAN ID that will be used for the EPG on the interface.
Mode | Defines whether the traffic should be tagged. The possible values are “regular” for tagged and “untagged” for untagged.
Pod | Pod where the switch or switches are.
Interface | Interface number.
Policy Group | Interface policy group name.

#### Workflow
![EPG to Interface Mapping](pics/image009.png)
 
### Port Channel Member Profile - LACP Timer
The goal of this task is to apply Fast LACP timer (1 sec) to vPC Interface Port Group.

#### Input Parameters
The input parameters for this task are defined in the tab “fast_lacp_timer”. Input parameters are described in the following table

Column | Description
---|---
vPC Interface Policy Group | The name of the existing vPC Interface Policy Name
Interface Profile | The name of the existing Leaf Interface Profile
Interface Selector | The name of the existing Interface Selector
Status | Values: created

#### Hardcoded Parameters
Hardcoded parameters are listed in the following table

Parameter | Name | Description
---|---|---
LACP_Fast_Pol | LACP Fast Policy Name | Fast LACP timer policy name
LACP_Fast_OverPol | LACP Fast Override Policy Name | Name of the Override fast LACP timer policy, applied to the PC policy group
txRate==fast | LACP Timer | Value of the Fast LACP timer (1sec)

#### Workflow
![LACP Timer](pics/usecase_35.png)

### Local APIC Username
The goal of this task is to create, delete, reset local APIC username and add SSH Key.

#### Input Parameters
The input parameters for this task are defined in the tab “local_apic_username”

Column | Description
---|---
Local APIC Username | Local APIC username
Local APIC User Password | Password for the local APIC username
Local APIC User Role | Define local APIC user role
Local APIC User Access | Define local APIC user access
Status | Values: created, deleted, reset
Local APIC User SSH Key | SSH Key for local APIC user
SSH Key Status | Values: created, deleted

#### Hardcoded Parameters
Hardcoded parameters are listed in the following table 

Parameter | Name | Description
---|---|---
userdomain-mgmt | Local APIC User Domain MGMT | APIC Domain mgmt
userdomain-common | Local APIC User Domain COMMON | APIC Domain common
userdomain-all | Local APIC User Domain ALL | APIC Domain all
username_ssh_key | Local APIC User SSH Key Name | Name of the applied SSH Key

Created local APIC Username have defined role and access across all domains.
Local APIC User SSH Key Name is combined by “Local APIC Username” and suffix “_ssh_key”.

#### Workflow
![Local Apic Username](pics/usecase_82_83.png)

### Switch OOB
The goal of this task is to change switch OOB IP address if required and allow external IP addresses to access to OOB network.
Default access will allow any traffic from anywhere (0.0.0.0/0). 
Custom defined access will allow https and ssh from defined subnet.

#### Input Parameters
Input parameters for this task are defined in the tab “switch_oob”.

Column | Description
---|---
Pod ID | Pod ID where switch is located
Node ID | Switch ID
Switch OOB IP Address in CIDR Format | New Switch OOB IP address
Status IP Address | Value: modified
Switch OOB Contract Type | Values: default, custom
Source IP Subnet in CIDR Format | External IP subnet

#### Workflow
![Switch OOB](pics/usecase_6.png)

### Ingress Based ACL's
The goal of this task is to set VRF Policy Control Enforcement Direction as Ingress.

#### Input Parameters
The input parameters for this task are defined in the tab “ingress_based_acl”

Column | Description
---|---
Tenant | Tenant Name
VRF | VRF Object Name

#### Hardcoded Parameters
Attribute “pcEnfDir” is set to “ingress”

#### Workflow
![Ingress Based ACLs](pics/usecase_9.png)

### Export Contracts
The goal of this task is to export contract, which can be used for intra-tenant or inter-tenant contract. 
Exported contract can see as imported contract in consumer tenant.

#### Input Parameters
Input parameters for this task are defined in the tab “export_contracts”.

Column | Description
---|---
Import Contract Object Name | Imported contract name in consumer tenant
Export Contract Object Name | Contract to be exported
Provider Tenant Name | Provider tenant from where contract is exported
Consumer Tenant Name | Consumer tenant where contract is exported
Export Status | Value == created

#### Workflow
![Export Contracts](pics/usecase_8.png)

### Taboo Contract
The goal of this task is to create Taboo Contract and assign to an EPG.

#### Input Parameters
Input parameters for this task are defined in the tab “taboo_contract”.

Column | Description
---|---
Tenant | Tenant Name
Application Profile | Application Profile Name
EPG | EPG Name
Taboo Contract Name | Taboo Contract Name
Taboo Subject Name | Taboo Subject Name
Taboo Filter Directives | Directives. Values are listed bellow
Filter Name | Filter Name
Filter Entry Name | Entry Name
Protocol | Protocol type
Destination Port Range From | Destination port range from
Destination Port Range To | Destination port range to

Directives values:
-	none
-	log
-	no_stats
-	log,no_stats

Tasks “filter” and “entry” are reused, where some parameters are hardcoded. See section “Hardcoded parameters”.

#### Hardcoded parameters
Hardcoded attributes: 
-	Ethernet Type (etherT) is “IP”
-	Source ports range (sFromPort and sToPort) have default values (Unspecified).

#### Workflow
![Taboo Contract](pics/usecase_7.png)

### Contract TCAM Optimized 
The goal of this task is to set Contract with optimized TCAM usage between EPG’s.

#### Input Parameters
Input parameters for this task are defined in the tab “contract_tcam_optimized”.

Column | Description
---|---
Tenant | Tenant Name
Application Profile | Application Profile Name
Consumer EPG | EPG which consume the contract
Provider EPG | EPG which provide the contract
Contract Name | Contract Name
Subject Name | Contract Subject Name
Filter Name | Filter Name
Filter Entry Name | Entry Name
Protocol | Protocol type
Destination Port Range From | Destination port range from
Destination Port Range To | Destination port range to

Tasks “filter” and “entry” are reused, where some parameters are hardcoded. See section “Hardcoded parameters”.

#### Hardcoded parameters
Hardcoded attributes in “Add Entry” task
-	Ethernet Type (etherT) is “IP”
-	Source ports range (sFromPort and sToPort) have default values (Unspecified).
Hardcoded attribute in “Add Filter to Subject TCAM Optimized” task, directives is set to no_stats, this parameter is required to enable TCAM optimization.

#### Workflow
![Contract TCAM Optimized](pics/usecase_10.png)

### Intra EPG Contract 
The goal of this task is to set Intra EPG contract.

#### Input Parameters
Input parameters for this task are defined in the tab “contract_intra_epg”.

Column | Description
---|---
Tenant | Tenant Name
Application Profile | Application Profile Name
EPG | EPG Name
Provider EPG | EPG which provide the contract
Contract Name | Contract Name
Subject Name | Contract Subject Name
Filter Name | Filter Name
Filter Entry Name | Entry Name
Filter Directives | Filer to Subject Directives. Values are listed bellow
Protocol | Protocol type
Destination Port Range From | Destination port range from
Destination Port Range To | Destination port range to

Tasks “filter” and “entry” are reused, where some parameters are hardcoded. See section “Hardcoded parameters”.

Directives values:
-	none
-	log
-	no_stats
-	log,no_stats

#### Hardcoded parameters
Hardcoded attributes:
-	Ethernet Type (etherT) is “IP”
-	Source ports range (sFromPort and sToPort) have default values (Unspecified).

#### Workflow
![Intra EPG Contract](pics/usecase_11.png)


## Example
In this example an ACI will be configured to provide connectivity to three ESXi hosts which
belong to the tenant ‘example’ and the VRF ‘vrf01’. These hosts are connected to leaf switches
with port channels, VPCs and orphan ports, and they use four VLANs: front, back, iscsi and
mgmt. For the VLANs front, back and mgmt the gateway should be configured on the ACI.
![Example Network](pics/image010.png)

VLAN Name | VLAN ID | Subnet | Gateway
--- | --- | --- | ---
front | 21 | 10.10.1.0/24 | 10.10.1.1
back | 22 | 172.16.1.0/24 | 172.16.1.1
iscsi | 23 | 192.168.100.0/24 | Not needed
mgmt | 24 | 172.16.100.0/24 | 172.16.100.1

### Prerequisites
It is assumed that the spreadsheet “autoaci_input.xls” is used to provide the input parameters to
the automation script and is in the folder /usr/local/autoaci/input, and the following objects have
already been configured.

Object | Name
--- | ---
Tenant | example
VRF | vrf01
Application Profile | ap01
Physical Domain | pd01
AAEP | aaep01

### Step 1. Policy Model Configuration
In this step Bridge Domains and EPGs are configured.
For this step the tab “policy_model” should be filled in as shown below. Then the script should
be executed with the command: `autoaci -f autoaci_input.xls -s policy_model -u
<user>`, where user is the username of the ACI user.
![Policy Model Configuration](pics/image011.png)

### Step 2. Configuration of port channels
For this step the tab “pc” should be filled in as shown below. Then the script should be executed
with the command: `autoaci -f autoaci_input.xls -s pc -u <user>`, where user is the
username of the ACI user.
![Configuration of port channels](pics/image012.png)

### Step 3. Configuration of VPCs
For this step the tab “vpc” should be filled in as shown below. Then the script should be
executed with the command: `autoaci -f autoaci_input.xls -s vpc -u <user>`, where user is the username of the ACI user.
![Configuration of VPCs](pics/image013.png)

### Step 4. Configuration of Orphan Ports

For this step the tab “orphan” should be filled in as shown below. Then the script should be
executed with the command: `autoaci -f autoaci_input.xls -s orphan -u <user>`, where
user is the username of the ACI user.
![Configuration of Orphan Ports](pics/image014.png)

### Step 5. Deployment of EPGs on Port Channels
For this step the tab “epg_to_pc” should be filled in as shown below. Then the script should be
executed with the command: `autoaci -f autoaci_input.xls -s epg_to_pc -u <user>`,
where user is the username of the ACI user.
![Deployment of EPGs on Port Channels](pics/image015.png)

### Step 6. Deployment of EPGs on VPCs
For this step the tab “epg_to_vpc” should be filled in as shown below. Then the script should be
executed with the command: `autoaci -f autoaci_input.xls -s epg_to_vpc -u <user>`,
where user is the username of the ACI user.
![Deployment of EPGs on VPCs](pics/image016.png)

### Step 7. Deployment of EPGs on VPCs
For this step the tab “epg_to_orphan” should be filled in as shown below. Then the script should
be executed with the command: `autoaci -f autoaci_input.xls -s epg_to_orphan -u
<user>`, where user is the username of the ACI user.
![Deployment of EPGs on Orphan Ports](pics/image017.png)


## Software requirements
The system has been tested with the following software versions:
- Docker Engine: 19.03.
- RHEL 8.
