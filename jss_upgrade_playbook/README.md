JSS Upgrade Ansible Playbook
============================

This playbook will update a clustered JSS using files from a manual installation.

### Requirements
- Ansible
- httplib2 on all remote hosts. (ubuntu: sudo apt-get install python-httplib2 osx: look it up based on your python install)

### Structure
				.
				├── group_vars
				│	├── all                 # Update this file to include the new/old versions you're working with. E.g. 9.7 to 9.72
				├── hosts                 # Host file for your environment. This example has production and test
				├── roles                 # Directory for server "roles"
				│	├── stop_services       # Load and stress tests
				│	│ ├── tasks               
				│ │ │   ├── main.yml      # .yml file to stop tomcat7 service 
				├── webapp                # Directory for Web App Role
				│	├── files                   
				│ │ ├── 9.7				# JSS Version 9.7
				│ │ │   ├── ROOT.war      # ROOT.war from Manual Install on version 9.7
				│ │	├── 9.72				# JSS Version 9.72
				│ │ │   ├── ROOT.war      # ROOT.war from Manual Install on version 9.72 
				│ │ ├── DataBase.xml      # DataBase.xml from YOUR server. Note: This is blank
				│ │ ├── log4j.properties  # log4j.properties from YOUR server. Note: This is blank
				│ ├── tasks                   
				│ │ ├── main.yml          # .yml file to perform the upgrade
				│ └── site.yml            # Main .yml file for playbook. 
				└──

### Steps to Take
 - Setup Ansible
 - Setup Hosts with Public Key Authentication 
 - Configure hosts file for your orginization
 - configure group_vars/all file to include version you are working with
 - Copy the ROOT.war from JSS Manual Install Download into appropriate directory. (Create one if you need)
 - Replace DataBase.xml with one from your environment /path/to/tomcat/webapps/ROOT/WEB-INF/xml/DataBase.xml
 - Replace log4j.properties with one from your environment /path/to/tomcat/webapps/ROOT/WEB-INF/classes/log4j.properties
 - Only run in a test environment until you are comfortable with changes that are being made.
 
 ### Test run of this playbook
``` 
ansible-playbook -i /path/to/hosts /path/to/site.yml --verbose --check
```
Update: 06-06-2015 Added logic to wait on upgrade to complete before continuing to upgrade additoinal webapps.
