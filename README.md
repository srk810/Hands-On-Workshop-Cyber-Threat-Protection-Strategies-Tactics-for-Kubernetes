# Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes
Hands-On Workshop: Cyber Threat Protection Strategies &amp; Tactics for Kubernetes

You should have been provided a cURL command to hook-up your cluster to Calico Cloud.

```
curl -s https://installer.calicocloud.io/XYZ_your_business_install.sh | bash
```

<img width="800" alt="Screenshot 2021-06-28 at 16 20 36" src="https://user-images.githubusercontent.com/82048393/123662025-f7a6da80-d82c-11eb-8ada-f92f9c6d24c0.png">


# Zone-Based Architecture

We will need to create a tier for our application which has a zone-based architecture:

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/zba/developer.yaml
```

The YAML file is only specifies the 'kind' of file to be a tier - as well as a tier name and priority order in which the tier should be evaluated:

<img width="468" alt="Screenshot 2021-06-28 at 16 37 54" src="https://user-images.githubusercontent.com/82048393/123664490-49e8fb00-d82f-11eb-855c-f144adaf3b58.png">

Once applied, you will see a hollow tier in the web user interface

<img width="826" alt="Screenshot 2021-06-28 at 16 38 23" src="https://user-images.githubusercontent.com/82048393/123664507-4eadaf00-d82f-11eb-91f8-e44d5102a7b3.png">


i) Demilitzarized Zone

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/zba/dmz.yaml
```

NB: Remember to add the Storefront application for your zone-based architecture to apply to

```
kubectl apply -f https://installer.calicocloud.io/storefront-demo.yaml
```

<img width="686" alt="Screenshot 2021-06-28 at 16 44 27" src="https://user-images.githubusercontent.com/82048393/123665393-25d9e980-d830-11eb-9a9c-eec55d0b5d97.png">

Now you should have your 1st 'staged' network policy in the newly-created tier:

<img width="808" alt="Screenshot 2021-06-28 at 16 49 51" src="https://user-images.githubusercontent.com/82048393/123666171-e1028280-d830-11eb-98d4-3d57b4c251a1.png">



ii) Trusted Zone

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/zba/trusted.yaml
```

<img width="483" alt="Screenshot 2021-06-28 at 16 51 56" src="https://user-images.githubusercontent.com/82048393/123666588-435b8300-d831-11eb-832e-e26ddd3ed72b.png">



iii) Restricted Zone

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/zba/restricted.yaml
```


<img width="509" alt="Screenshot 2021-06-28 at 16 57 31" src="https://user-images.githubusercontent.com/82048393/123667332-f5934a80-d831-11eb-8d69-6fd8d99548c9.png">



Finally, you should have 3 zones within your development tier - we call this a zone-based architecture:


<img width="821" alt="Screenshot 2021-06-28 at 16 58 34" src="https://user-images.githubusercontent.com/82048393/123667543-27a4ac80-d832-11eb-93ad-d3bfd0426773.png">



# Anonymization Attacks

EJR VPN feed targets major VPN providers and their infrastructure used in anonymization activity over the internet. The feed is updated bi-monthly, which helps network security teams to stay on top of threats from such anonymizing infrastructure and detect them early in the enumeration phase.

```
kubectl apply -f https://docs.tigera.io/v3.7/manifests/threatdef/ejr-vpn.yaml
```

The Tor Bulk Exit feed lists available Tor exit nodes on the internet which are used by Tor network. The list continuously updated and maintained by the Tor project. An attacker using Tor network, is likely to use one of the bulk exit nodes to connect to your infrastructure. The network security teams can detect such activity with Tor bulk exit feed and investigate as required.

```
kubectl apply -f https://docs.tigera.io/v3.7/manifests/threatdef/tor-exit-feed.yaml
```

Now, you can monitor the Dashboard for any malicious activity. The dashboard can be found at Calico Enterprise Manager, go to “kibana” and then go to “Dashboard”. Select “Tor-VPN Dashboard”.


```
kubectl get globalthreatfeeds 
```

<img width="766" alt="Screenshot 2021-06-29 at 09 21 09" src="https://user-images.githubusercontent.com/82048393/123763218-78141c80-d8bb-11eb-83b9-9faab997ec6d.png">


The Tor-VPN dashboard helps network security teams to monitor and respond to any detected activity by Tor and VPN feeds. It provides a cluster context to the detection and shows multiple artifacts e.g. flow logs, filtering controls, a tag cloud and line graph to analyze the activity and respond faster. 


# Network Policy based on Threat Feeds

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/networkpolicies/feodo-tracker.yaml
```

```
kubectl get globalnetworksets threatfeed.feodo-tracker -o yaml
```

We will need to create a 'security' tier for our threat feed policies

```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/networkpolicies/security.yaml
```

Create the policy to block traffic that matches the label specified in the threat feed
```
kubectl apply -f https://raw.githubusercontent.com/tigera-solutions/Hands-On-Workshop-Cyber-Threat-Protection-Strategies-Tactics-for-Kubernetes/main/networkpolicies/block-feodo.yaml
```

# HoneyPods






# Anomaly Detection
