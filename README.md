# zabbix-vmwareesx
This is a template which can be used to monitor oVirt or libvirt virtualization hosts from within Zabbix.

The template was exported from Zabbix version 2.2.4
Tested with ESXi 5.5.0 build 1881737

##How to use:
1. Enable SNMP on the VMWARE ESX-hosts:
  - connect with SSH to the ESX host and execute the following commands:
    - replace <community> with your SNMP-community
    - `esxcli system snmp set --communities <community>`
    - `esxcli system snmp set --enable true`
2. Enable SNMP trough the firewall on the VMWARE ESX-hosts:
  - connect with SSH to the ESX host and execute the following commands:
    - `esxcli network firewall ruleset set --ruleset-id snmp --allowed-all true`
    - `esxcli network firewall ruleset set --ruleset-id snmp --enabled true`
    - `/etc/init.d/snmpd restart`
	
3. Import the template in Zabbix:
  - connect to your Zabbix Web-GUI and go to Configuration - Templates
  - click on Import
  - choose the file and click on Import
	
4. Add your VMWARE ESX-hosts to Zabbix:
  - connect to your Zabbix Web-GUI and go to Configuration - Hosts
  - for each host: click on Create Host
  - On the host tab:
    - enter a host name 
    - add an SNMP-interface with your ESX-hosts IP and/or hostname
  - On the templates tab:
    - add template "Template SNMP ESX"
  - On the Macros tab:
    - enter macro {$SNMP_COMMUNITY} with <community> as value
    - enter macro {$SNMP_PORT} with your SNMP-port as value (default 161)
  - click Save

5. Wait for the discovery to fire that creates all items, graphs and triggers
