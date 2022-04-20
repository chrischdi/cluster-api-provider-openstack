```
gcloud compute networks list --filter="name~'.*capo.*'"
gcloud compute networks subnets list  --filter="name~'.*capo.*'"
gcloud compute firewall-rules list --filter="name~'.*capo.*'"
gcloud compute routers list --filter="name~'.*capo.*'"
gcloud compute disks list --filter="name~'.*capo.*'"
gcloud compute images list --filter="name~'.*capo.*'"
gcloud compute instances list --filter="name~'.*capo.*'"
```

```sh
region=us-east4
zone=$region-a

for instance in $(gcloud compute instances list --filter="name~'.*capo.*'" --format json | jq '.[].name'); do
  gcloud compute instances delete $instance --zone $zone
done
gcloud compute images delete capo-e2e-controller-image capo-e2e-worker-image
gcloud compute disks list --filter="name~'.*capo.*'"
gcloud compute disks delete capo-e2e-disk --zone $zone
gcloud compute routers delete capo-e2e-myrouter --region $region
gcloud compute firewall-rules delete capo-e2e-mynetwork-allow-http capo-e2e-mynetwork-allow-icmp capo-e2e-mynetwork-allow-internal capo-e2e-mynetwork-allow-neutron capo-e2e-mynetwork-allow-ssh
gcloud compute networks subnets delete capo-e2e-mynetwork --region $region
gcloud compute networks delete capo-e2e-mynetwork
```