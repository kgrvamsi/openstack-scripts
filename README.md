# Welcome to the OpenStack Install Scripts project on GitHub

This project aims to covert the steps in the OpenStack Install guide into useful/simple scripts. The goal is not to provide a complete automation solution. But the idea is to have simple building blocks which can help developers and enthusiasts experiment with OpenStack in a much simpler fashion.

#### Platform supported - Ubuntu
#### OpenStack version - Juno
#### OpenStack services installed ####
The following OpenStack services are installed as part these scripts:

1. Keystone (Identity)
2. Glance (Image)
3. Nova (Compute)
4. Neutron (Networking)
5. Horizon (Dashboard)
6. Ceilometer (Telemetry)

#### Prerequisites ####
Git packages must be installed on your Ubuntu server. This is needed to checkout the scripts to your Ubuntu server.

#### Important Notes ####
1. The installation installs only Open vSwitch packages by default. Installing Linux bridge can be done using a utility script provided in `util` directory.
2. The Nova configuration includes a setting to use `AllHostsFilter` as the default filter for scheduler. If you want the default filters of Nova, remove this entry and restart Nova services on the controller.
3. Many scripts detect the OpenStack Node Type automatically using `util/detect-nodetype.sh` script.

## How to use the scripts step by step ##

1. Update lib/config-parameters.sh script 
   - Change the hostname to be used for controller. This name will be used in all configuration files. 
   - You will need to use this name to update the /etc/hosts file for correct lookups. The `util/update-etc-hosts.sh` script can be used for this purpose.
   - Change passwords as necessary 

2. Install common packages
   - Execute `sudo bash install.sh common`
   - It is a good idea to reboot the system since kernel packages may have been updated.

3. Install OpenStack packages depending upon the type of the node
   - If the node is of type controller, execute `sudo bash install.sh controller`
   - If the node is of type compute, execute `sudo bash install.sh compute`
   - If the node is of type networknode, execute `sudo bash install.sh networknode`
   - If the node is of type allinone, execute `sudo bash install.sh allinone`
   - **Note - during the installation of MariaDB, you will be required to enter DB password manually**

4. Configure OpenStack packages using `sudo bash configure.sh`. The **node type** is detected automatically
   - **Note - during the configuration of MariaDB, you will be required to confirm few DB clean up operations manually** 

## Updating IP Address ##

The install scripts use a name for the controller (defined in `config-parameters.sh` script). This name needs to be updated in the /etc/hosts file. Also to view the VNC console of an instance, it is convenient to use IP address in the Nova configuration file. All these changes can be done using `util/update-ip.sh` script. 

Usage: `sudo bash util/update-ip.sh <controller-host-name> <controller-ip>`. 
The second parameter is used only for compute nodes.

## Removing OpenStack packages ##

You can remove all the OpenStack packages using `sudo bash remove.sh`. The **node type** is detected automatically and the corresponding packagesa are removed.

## Restarting OpenStack services ##

Restarting OpenStack services is needed at times - especially when config file entries are changed. You can execute `sudo bash restart.sh` to do this. The **node type** is detected automatically.

## Updating /etc/hosts file ##

Since all configuration uses the name of the controller host, it is important to update `/etc/hosts` file on all the node to map an IP address to the controller host name. You can use the `util/update-etc-hosts.sh` script for this purpose.

