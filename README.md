# Deploying Cloudbees on PKS
Instructions to get Cloudbees Core deployed on Kubernetes Cluster operated by PKS

nstall Cloudbees Core on PKS ( assumes NSX T Load Balancers are available for service creation)


* Ensure you have a storage class created by the name 'default', this storage class will be used by the Persistent Volume claims needed for stateful sets.
 
	· To add a storage class store the below YAML file as pks-storageclass.yaml
	```yaml\
    kind: StorageClass
		apiVersion: storage.k8s.io/v1
		metadata:
		  name: default
		  annotations:
		    storageclass.kubernetes.io/is-default-class: "true"
		provisioner: kubernetes.io/vsphere-volume
		parameters:
		  diskformat: thin
		  
 
	· `Kubectl apply -f pks-storageclass.yaml`
 
* Download the latest Cloudbees installer for Kubernetes from the below link:
 
https://downloads.cloudbees.com/cloudbees-core/cloud/latest/

 
 
* Untar the file, and follow further instructions to installing cloudbees core as per the link below:

 
     https://go.cloudbees.com/docs/cloudbees-core/cloud-install-guide/kubernetes-install/
     
* If you prefer service type loadbalancer to Ingress, edit the service for the pod ‘cjoc’ and replace service Type from ‘ClusterIp’ to ‘LoadBalancer’
 
	`kubectl edit service cjoc`
 
* Find the external IP address of the ‘cjoc’ service
	`external_IP=$(kubectl get svc | grep cjoc | awk '{print $4}')`
 
* Access Cloudbees Core Operations Center console by going to `$external_IP/cjoc`
