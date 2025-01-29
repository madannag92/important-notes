KUBECTL COMMANDS
let say two pods are running and one pod is crashloop back off and other pod is running. but the overall pod status when you hit command kubectl get pods -
-the answer is crashloopbackoff
-------------------
To check the version:
kubectl version
kubectl version --output=yaml
Kubectl version --client
--------------------
To check info about the cluster
kubectl config view
-------------------
To run a Pod
kubectl run <pod name> --image <image name>
kubectl run myngix --image nginx
------------------
To create a deployment
kubectl create deployment <name> --image <image name>
kubectl create deployment mynginx --image nginx
-------------------
kubectl logs -n <namespace> -f <current-pod-name> --previous this will print logs of previous pod which was terminated.
-------------------
To scale the deployment (increase replicas)
kubectl scale deployment <deployment name> --replicas <no of replicas>
kubectl scale deployment mynginx --replicas 2
------------------
To check all running services, pods, etc.
kubectl get all
------------------
To get the get details from the a particular namespace
kubectl get all -n <namespace name>
------------------
To get the internal components running
kubectl get pods -A 
kubectl get pods -A -owide
------------------
To check all the running services
kubectl get services
-----------------
To check all the running pods
kubectl get pods
// with extra details
kubectl get pods -o wide
-----------------
To check all the running node.
kubectl get nodes
-----------------
To check all the replicaset
kubectl get replicaset
----------------
To check all the namespaces
kubectl get namespaces
---------------
To get all the API resources
kubectl api-resources
---------------
To delete the deployment
kubectl delete deployment <deployment-name>
---------------
To delete the pods
kubectl delete pod <pod-name>
---------------
To delete evicted pods
kubectl delete pod --field-selector="status.phase==Failed"
---------------
To get logs of a pod
kubectl logs <pod-name>
---------------
To check logs or sh/bash of a container inside a pod. That if pods have multiple container an we have enter inside a container
kube exec -it <pod-name> -c <container-name> -- <bash command>
kube exec -it multi-container -c nginx-container -- curl localhost
kube exec -it multi-container -c nginx-container -- sh
kubectl logs multi-container -c nginx-container
---------------
To get inside the pod
kubectl exec -it <pod name> -- sh
kubectl exec -it nginx -- sh
---------------
Get a deep details/state chnages about a pod
kube describe pod <pod -name>
---------------
To watch the pods (watch refresh every few seconds)
kubectl get pods -w
---------------
To check the cluster are avilable
kube config get-contexts
We can create namespace by
kubectl create namespace <name>
kubectl create namespace dev
To do a dry nun and get the output as Yaml
kubectl create namespace test-name --dry-run=client -o yaml
---------------
To edit the deployment (deployment file)
kubectl edit deployment <deployment name>
---------------
To delete all the pods
kubectl delete pods --all
Apply to a particular namespace
kubectl apply -f <config file name> --namespace=<namespace name>
---------------
Persistent Volume
Get all the PersistentVolume
kubectl get pv
---------------
Get all the PersistentVolumeClaim (tied to a namespace)
kubectl get pvc
---------------
To chnage default/active namespace
kubectl config set-context --current --namespace=<namespace name>
kubectl config current-context -> to check which current config file am I using
kubectl config use-context <my-cluster-name> -> to be used to switch between other configs with the full name to be mentioned starting with arn and all
kubectl config get-contexts -> to show all the contexts
---------------
To get the details of a particular namespace
kubectl get all -n <namespace name>
--------------------------------------------------------------------------------
helm upgrade --install aws-corretto-nginx ./helm-deployment -n cb-artifact-server -f ./helm-deployment/values-prod.yaml --set image.tag=14 -> to upgrade the helm chart    the one in the yellow is the namespace and after -f it is the file location and also while doing the helm upgrade u should be in the samelocation of the folder

helm upgrade --install nexus-repo-test deployment/nexus-repo -n sonatype-nexus-test -f deployment/values-staging.yaml --atomic --timeout 7m --set 'image.tag=3.56.0-25'

while checking for the helm issues, also use the helm cmd's for getting to know the issue.

If the node is not coming up in the cluster, restart the kubelet service in the node i.e ec2 instance which should fix the issue

C:\Users\nagamadan.en\.kube\config - is the location for kubeconfig file
journalctl -u kubelet -f - to find out the logs of the kubelet
journalctl -u docker -f - to find out logs of the docker
use to remove incomplete images docker rmi -f $(docker images -aqf "dangling=true"). this removes unbuilt images without touching the docker caches.
Docker images sits in /var/lib/docker location
sudo yum install docker-ce-26.1.3 docker-ce-cli-26.1.3 containerd.io -> to upgrade docker version and client as well
yum list docker-ce --showduplicates | sort -r -> to list docker versions
df -kh /var/lib/docker
Location of space issue in static ec2 instance is /var/lib/docker/overlay2
docker image prune -a
Docker system df = shows types of docker images, containers and so on
Docker system prune - removes all images, containers(never use without checking)
docker buildx prune --filter=until=72h -> clears the system cache in the ec2 instance
docker container prune - to prune the containers
Docker image prune -a -> delete the images which are not being used
Docker image prune -a --filter "until=72h" - deletes all the images after 72 hours timestamp
docker buildx prune --filter=until=168h -> clean build cache keeping a week worth of cache
Cron tab -l -> lists all crn jobs
Cron tab -e -> edit all the existing cron cmd
/var/log/cron -location for cron logs
Systemctl status crond - to check the status
cat cron | grep "Mar 20" 
docker ps -a |wc -l 
Kubectl drain <node-name> - before deleting the node
eksctl create nodegroup --cluster my-cluster --region region-code --name my-nodegroup --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 4
kubectl run nginx --image nginx -n vv-perf-master - sample job run to check if the image is being pulled
sudo chmod 777 /var/run.docker.soc - use this for setting temp root privilieges in a docker agent
docker exec -it --user root <image name> /bin/bash - to run as root 
Docker run -it <imagename> /bin/bash -> try this if u have the image to login inside the image
Kubectl get pods --all-namespaces -o wide | grep ip-172.17.209.241 -> gets filtered which ever node ip pods are running
Kubectl describe -n hla-prod-> describe the namespace
Kubectl get storageclass - gets all the types of storageclasses
Kubectl cordon <node-ip>- scheduleding will be disabled
kubectl scale deploy hla-api --replicas=1 -n hla-prod = to scale the deployment  (yellow is the deployment file name)
kubectl get node,statefulset,pod,svc,ingress,endpoints,cm,pvc,pv -o yaml -n cloudbees-cdro-dev  > cloudbees-details.yaml
>kubectl top pod vg-0 -n radit-ci-vg
kubectl describe namespace <ns-name> -> to describe the config of namespace
kubectl describe PodMetrics vg-0 -n radit-ci-vg
kubectl exec -it vg-0 -n radit-ci-vg /bin/sh - to go inside the shell
kubectl delete pod vg-0 --force -n radit-ci-vg 
kubectl get secrets -n mlspot-dev - shows the secret of particular namespace
kubectl describe secret default-generated-token -n mlspot-dev ->this is the token name in the namespace
kubectl describe sa cjoc -n radit-ci-rit-ha-1 -> gives service account details attached to the namespace
Kubectl logs <podname> -n <namespace> -> to get the logs of the pod
kubectl cp vv-jmeter-hjsds:/tmp/workspace/Performance/performance_test_parallel/jmeter.log ./jmeterlatest.txt -n radit-ci-vg ->this cmd helps us copy the file from lens to local system. Bold is pod name
kubectl cp sonarqube-sonarqube-dce-app-74b587f9fb-77s4r:/opt/sonarqube/logs ./sonar.txt -n sonar-dev -> one more example
aws ssm get-parameter --name /aws/service/eks/optimized-ami/1.25/amazon-linux-2/recommended/image_id --region us-east-1 --query "Parameter.Value" --output text ->this is the cmd to check in cli which ami is suitable for eks cluster version. Where 1.25 is cluster version which will change according the cluster version which we are upgrading to.
aws eks update-cluster-version --region region-code --name my-cluster --kubernetes-version 1.30
eksctl get clusters - shows all clusters
helm rollback ap-operations-service 32 -n vv ->32 is the version and  ap-operations-service is the helm chart name
docker run -it --user root --entrypoint /bin/bash 154919775133.dkr.ecr.us-east-1.amazonaws.com/radit-jenkins-agent-sonar:T23D-8238-15
docker run -it --entrypoint /bin/bash 154919775133.dkr.ecr.us-east-1.amazonaws.com/radit-jenkins-agent-sonar:T23D-8238-15
docker run -it --user root --entrypoint /bin/bash 154919775133.dkr.ecr.us-east-1.amazonaws.com/jenkins-agent-sonar:T23D-8238-15

docker run -it --user root --entrypoint /bin/bash 154919775133.dkr.ecr.us-east-1.amazonaws.com/jenkins-agent-r:T23D-85121-5 -> direct command for running the jenkins agent

Making the pv and pvc from readwrite once to readwrite many
        1.first got the yml file of the pv of the claimed pvc make the changes to readwritemany from readwriteonce 
	2. Also change the persistentvolumeclaim from delete to retain and save the pv yml file
	3. Stop the controller
	4. Delete the pvc
	5. Go back to the pv of the controller and delete the old claimed pvc section and then start the controller and it dynamically allocate the new pvc.

When ever there is a issue with mapping pvc's on an agent while running the agent make sure to check in route 53 the hosted zone of the efs is added or not
And also important thing is that we need to make sure we add the A records in the hosted zone so the connection is completed

If anytime a agent is causing any issue create a pod in any namespace and login to that and fix the issue
Use the cmd ->kubectl exec -it static-web -- /bin/bash static web is the pod name here

su jenkins
    2  cd /etc/sonar
    3  ls
    4  rm -rf jre
    5  ls
    6  mkdir jre/bin
    7  mkdir -p jre/bin
    8  ln -s /usr/bin/java /jre/bin/java
    9  ln -s /usr/bin/java jre/bin/java ln -s /usr/bin/java /opt/sonar-scanner/jre/bin/java


   10  sonar-scanner
   11  chmod -R 777 jre
   12  su - jenkins
   13  history

---------------------------------

Sample PVC yml file
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: hla-staging
  namespace: hla-staging
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Gi
  storageClassName: aws-efs
  volumeMode: Filesystem
