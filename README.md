<!-- GETTING STARTED -->
## Getting Started

MetalLB Installation on onpremise K8s Cluster

### Prerequests

> kubectl config get-contexts

Please select required kubeconfig file.



### Installation

1. Clone Repo
   ```sh
   git clone https://gitlab.pudo.local/manas/devops.git
   ```
2. Change Directory
   ```sh 
   cd kubernetes/metallb
   ```
3. Create Namespace
   ```sh
   kubectl create ns metallb-system
   ```
4. Create Config Yaml
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
   namespace: metallb-system
   name: config
   data:
   config: |
       address-pools:
       - name: my-ip-space
       protocol: layer2
       addresses:
       - 10.255.134.160-10.255.134.170
   ```
5. Apply YAML files
   ```sh
   kubectl apply -f config.yaml
   kubectl apply -f metallb.yaml
   ```
6. Create metallb secret
   ```sh
   kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
   ```

<!-- USAGE EXAMPLES -->
## Usage

Service deploy edildikten sonra herhangi bir nginx image ile test edilebilir.
1. Create Example Nginx Deployment
   ```sh
   kubectl create deployment nginx --image=nginx
   ```
2. Expose Your Deployment
   ```sh
   kubectl expose deploy nginx --port 80 --type LoadBalancer
   ```
3. Result

| NAME          | TYPE          | CLUSTER-IP       | EXTERNAL-IP   | PORT(S)                     | AGE           |
| ------------- | ------------- | ---------------- | ------------- | --------------------------- | ------------- |
| nginx         | LoadBalancer  | 172.19.2219.173  | 10.255.134.16 | 80:30383/TCP,443:32503/TCP  | 2m            |

> Eğer external ip belirlenen configmap te belirlenen aralıktan almıyorsa ya da "Pendin" statüsünde bekliyorsa lütfen konfigürasyonu tekrar kontrol edin!

<!-- CONTACT -->
## Contact

Adesso DevOps - devops@adesso.com.tr
