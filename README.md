# gwm-environment
This Ansible configuration file to build Google Workspace Migrate required servers on Google Cloud Project

# Preparing

1- Creat a project on GCP
	
	1- Keep the project ID in your notes.
	2- Enable Compute Engine API
	
2- Install Ansible on your machine Check this Url to install Ansible
	
	https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
	
3- Check this requisites for GCP module with Ansible
	
	https://docs.ansible.com/ansible/latest/scenario_guides/guide_gce.html#requisites
		
3- Create a Service account to authenticate Ansible with your project

	Only need this roles :
	
	- Compute Admin
	- Compute Network Admin

4- Creat a Key for your service account with the following name (save it at keys folder): 
	
	gwm-ansible-key.json

# Changing varibales

A- Open gwm-servers.yaml file with your preferred editor

B- Navigate to Vars section

C- Check the following:

1- Your GCP Project ID 

		project: PROJECT_ID
				
2- Default Region and Zone 
				
        default_region: europe-west1
        default_zone: "{{ default_region }}-b"
		
3- Check Asnible authentication method and service account key path

	auth_kind: serviceaccount
        auth_key: ./keys/gwm-ansible-key.json
		
4- Choose Windows image that you want to use on all of your GWM servers

	windows_image: projects/windows-cloud/global/images/windows-server-2016-dc-v20220314

	---

	Note: Recommended operating system—Microsoft Windows Server 2016.
	https://support.google.com/workspacemigrate/answer/9222864

	You can find all windows server images using the following command on Cloud Shell or using Google Cloud SDK:

	gcloud compute images list --uri | grep windows-server
		
5- Specify machines types and disks size
       
	  
	Check the following link for requierd and recommended specs for your machines:
	https://support.google.com/workspacemigrate/answer/9222864
	
	
	We need at least 4 Windows server machines:
	
	Check the following links for more about Machine types on GCP:
	https://cloud.google.com/compute/docs/machine-types
	https://gcpinstances.doit-intl.com/
	
	# ---GWN Platform server---
	Recomended: Platform server—One Windows Server with at least 4 cores, 16 GB of RAM, and 200 GB SSD.
			
	# Machine type for GWM Platform Server
        ps_machine_type: e2-standard-4
        # SSD disk size for GWM Platform Server
        ps_disk_size: 200
        
	Recomended for Databases:
	Database servers—2 Windows Servers, each with at least 16 cores, 64 GB of RAM, and a separate SSD for the database data.
		
        # ---GWM MySQL Database---
		Recomended: recommend approximately 1 TB per 100 million objects that you want to scan and migrate
		
        # Machine type for GWM MySQL Database
        mdb_machine_type: e2-standard-16
        # Your SSD disk size for GWM MySQL Database
        mdb_disk_size: 50

        # ---GWM CouchDB Database---
		Recomended: recommend approximately 1 TB per 40 million objects that you want to scan and migrate.
			
        # Machine type for GWM CouchDB Database
        cdb_machine_type: e2-standard-16
        # SSD disk size for GWM CouchDB Database
        cdb_disk_size: 50

        # ---GWM node server---
		Recomended: At least one Windows server, each with at least 4 cores, 32 GB of RAM, and 200 GB SSD
			
        # Machine type for GWM noder servers
        node_machine_type: e2-highmem-4
        # Your SSD disk size for GWM noder servers
        node_disk_size: 200
		
		
        # Give your GMW node server ID that you want and you can chenge it to create node servers as needed
        node_id: 1
		
# Running the playbook

Navigate to your playbook and run the following command:
	
	ansible-playbook gwm-servers.yaml

