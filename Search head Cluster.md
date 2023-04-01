**Search Head Cluster**


**Search Peer 1:**

/opt/splunk/bin/splunk init shcluster-config -auth admin:longgone -mgmt_uri https://10.0.16.234:8089 -replication_port 9000 -replication_factor 3 -conf_deploy_fetch_url https://10.0.29.98:8089 -secret longgone -shcluster_label shcluster1

**Search Peer 2:**

/opt/splunk/bin/splunk init shcluster-config -auth admin:longgone -mgmt_uri https://10.0.25.229:8089 -replication_port 9000 -replication_factor 3 -conf_deploy_fetch_url https://10.0.29.98:8089 -secret longgone -shcluster_label shcluster1

**Search Peer 3:**

/opt/splunk/bin/splunk init shcluster-config -auth admin:longgone -mgmt_uri https://10.0.29.3:8089 -replication_port 9000 -replication_factor 3 -conf_deploy_fetch_url https://10.0.29.98:8089 -secret longgone -shcluster_label shcluster1

 
**Any Search Peer:**

/opt/splunk/bin/splunk bootstrap shcluster-captain -servers_list "https://10.0.16.234:8089,https://10.0.25.229:8089,https://10.0.29.3:8089" -auth admin:longgone


![MicrosoftTeams-image](https://user-images.githubusercontent.com/125336591/229308568-3f2e2314-b02f-48d1-9b1b-e3a9918b65f2.png)
