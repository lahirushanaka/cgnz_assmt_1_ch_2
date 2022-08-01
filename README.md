# cgnz_assmt_1_ch_2
# Minikube on AWS

### Create AMI EC2 instance using below link
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html

### Install docker
```
sudo yum update -y
sudo yum install docker -y
sudo systemctl status docker
sudo systemctl enable --now docker
```

### Install Kubectl
```
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
mv kubectl /bin/kubectl
chmod a+x /bin/kubectl
```

### Install Minikube
```
https://aws.plainenglish.io/running-kubernetes-using-minikube-cluster-on-the-aws-cloud-4259df916a07
https://minikube.sigs.k8s.io/docs/start/
```
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/bin/minikube
sudo yum install conntrack-tools -y
```

### Start Minikube
```
minikube start --vm-driver=docker
```

### Test the setup
```
kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
kubectl expose pod hello-minikube --type=NodePort

```
### Deploy deployement and Service
```
kubectl apply -f nginx.yaml
```
### Deploy Ingress
```
kubectl apply -f ingress.yaml
```
### Get access from Remote Machine, Im using Amazon Linux 2 AMI, So following commands related to that distribution.
```
sudo amazon-linux-extras install -y nginx1
```
### Add below line on /etc/nginx/nginx.conf (AWS provided DNS is bit longer that default value, We have to change)
```
server_names_hash_bucket_size 128;
```
### Create new file /etc/nginx/conf.d/minikube.conf add below content
```
server {
            listen       80;
                listen  [::]:80;
                    server_name  << AWS DNS NAME>>;
                                location / {
                                                proxy_pass http://ec2-54-151-221-250.ap-southeast-1.compute.amazonaws.com:80;
                                                }
} 
```
### Restart Nginx
```
sudo service nginx start
```
