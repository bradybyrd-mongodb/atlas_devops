#-----------------------------------#
#  Devops API examples for Atlas    #

#### Python script modules for interacting with the Atlas API. ####

Basic Operation:

- First modify the rest_settings.json to include your information
The first 5 items are only used if you need to write information back to a MongoDB database.
```json
  "uri" : "mongodb+srv://mycluster-vmwqj.mongodb.net",
  "cluster_name" : "mycluster",
  "database" : "billing",
  "collection" : "paths",
  "username" : "admin",
  "password" : "<secret>",  
```
Right now, only the billing invoices methods need to write back

- The next few settings specify API access
```json
"project_id" : "5d4d7ed3f2a30b18f4f88946",
"org_id" : "5e384d3179358e03d842ead1",
"api_key" : "------------------------------------------",
"api_public_key" : "XXXXLCQ",
"base_url" : "https://cloud.mongodb.com/api/atlas/v1.0",
```
Your project and org ids can be picked out of any atlas URL, like this:
https://cloud.mongodb.com/v2/5d4d7ed3f2a30b18f4f88946#clusters
The project ID is the 5d4d... string.  From an org level url, you can get the org ID:
https://cloud.mongodb.com/v2#/org/5e384d3179358e03d842ead1/projects

At the project level (or org level if you need project creation, priavteLinks etc), create an API key pair.  This will give you a public and private key.  The settings file uses a hashed version of this for security (weak, but something).  

- To hash your API keys:
  from the command line, run the encrypt method, like this:
```bash
  python3 atlas_rest.py action=encrypt secret="<publickey>:<privatekey>"

# This will give you the hashed string to put in the api_key field.

# Now you should be able to test it:

  python3 atlas_rest.py action=project_info

# If that fails, first thing to check is the network access which is specific to the api key pair.
```

#### API Methods Examples ####
```bash

# Organization Info (uses the organization id from the settings file)
  python3 atlas_rest.py action=org_info

# Project Info (uses the project id from the settings file)
  python3 atlas_rest.py action=project_info

# Cluster Info (uses the project id from the settings file)
#  Returns information on all clusters in the project
  python3 atlas_rest.py action=cluster_info

#  To get detailed information on a single cluster:
  python3 atlas_rest.py action=cluster_info name=M10BasicAgain

# [Start or pause a cluster](https://docs.atlas.mongodb.com/pause-terminate-cluster/):
  python3 atlas_rest.py action=update_cluster name=MigrateDemo2 data='{"paused" : false}'

# [Reconfigure a cluster](https://docs.atlas.mongodb.com/scale-cluster/):
  python3 atlas_rest.py action=update_cluster name=M10BasicAgain data='{"labels" : [{"key": "department", "value" : "CodeWizards"},{"key": "owner", "value":"Brady Byrd"}]}'
```

#### Usage API ####
```bash
org_info
org_projects
project_detail
project_users
alert_settings
private_links
private_link_svc_detail
private_link
create_private_link_svc
create_private_link
azure_cli_command
gcp_cli_command
create_kms_encryption
kms_encryption
billing
billing_invoice
department_accounting
user_add
org_users
database_users
db_user_audit
user_audit
cluster_audit
atlas_monitoring
cluster_info
create_cluster
update_cluster
resume
update_cluster_labels
create_online_archive
online_archives
search_indexes
search_index
search_hw_metrics
search_metrics
logs
```