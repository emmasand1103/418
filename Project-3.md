# Creating the VMs  
gcloud config list project    
gcloud config list zone    
gcloud compute instances create www1 –zone=us=central1-a –tags=network-lb-tag –machine-type=e2-small –image-family=Debian-11 –image-project=Debian-cloud  
gcloud compute instances create www2 –zone=us=central1-a –tags=network-lb-tag –machine-type=e2-small –image-family=Debian-11 –image-project=Debian-cloud  
gcloud compute instances create www3 –zone=us=central1-a –tags=network-lb-tag –machine-type=e2-small –image-family=Debian-11 –image-project=Debian-cloud  
gcloud compute firewall-rules create www-firewall-network-lb  --target-tags network-lb-tag --allow tcp:80  
  
*in all of the virtual machines*  
  sudo apt install apache2  
  sudo systemctl restart apache2  
  systemctl status apache2  
    
gcloud compute instances list  
curl http://34.134.93.86  
curl http://34.30.130.177  
curl http://34.171.161.243  

# Task 3:  
Gcloud computer addresses create network-lb-ip-1 –region us-central1    
gcloud compute http-health-checks create basic-check  
gcloud compute target-pools create www-pool --region us-central1 --http-health-check basic-check  
gcloud compute target-pools add-instances www-pools --instances www1,www2,www3  
gcloud compute forwarding-rules create www-rule  --region  Region --ports 80  --address network-lb-ip-1 --target-pool www-pool  
  
# Task 4:   
Gcloud compute forwarding-rules describe www-rule –region us-central1  
:loop curl -m1 34.132.176.172 goto loop  

# Task 5:   
Created instance template and instance group on Google Console https://cloud.google.com/load-balancing/docs/https/ext-https-lb-simple#console   
gcloud compute firewall-rules create fw-allow-health-check \  
  --network=default \  
  --action=allow \  
  --direction=ingress \  
  --source-ranges=130.211.0.0/22,35.191.0.0/16 \  
  --target-tags=allow-health-check \  
  --rules=tcp:80  
gcloud compute addresses create lb-ipv4-1 --ip-version=IPV4 --global  
gcloud compute addresses describe lb-ipv4-1 --format="get(address)" --global  
gcloud compute health-checks create http http-basic-check --port 80  
gcloud compute backend-services create web-backend-service --protocol=HTTP --port-name=http --health-checks=http-basic-check --global  
gcloud compute backend-services add-backend web-backend-service --instance-group=lb-backend-group --instance-group-zone=Zone--global  
gcloud compute url-maps create web-map-http --default-service web-backend-service  
gcloud compute target-http-proxies create http-lb-proxy --url-map web-map-http  
gcloud compute forwarding-rules create http-content-rule --address=lb-ipv4-1 –global --target-http-proxy=http-lb-proxy --ports=80  
