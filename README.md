# toktik-k8s
###  Project directory tree
```
├── toktik-backend
│   ├── src
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-frontend
│   ├── Dockerfile
│   └── package.json
├── toktik-encode-worker
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-chunk-worker
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-thumbnail-worker
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-encode-scheduler
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-process-scheduler
│   ├── Dockerfile
│   └── requirements.txt
├── toktik-k8s
│   ├── *.yml
```
The above is the project directory tree if you clone all of our repositories. Fortunately, you don't need to. Because all images were pushed to `ghcr.io`

The only repo you need to clone is `toktik-k8s` which contains all necessary deployment files and run the code below

before running the following code make sure to install `helm` and `kubectl` 


To set up kubernetes, run (in toktik-k8s):
```bash
helm upgrade --install traefik traefik/traefik
kubectl create secret docker-registry ghcr-login-secret \
 --docker-server=https://ghcr.io \
 --docker-username={username} \ 
 --docker-password={personal_access_token}I
kubectl apply -f .
```
The code above install trafik proxy using helm then create a k8s secret for pulling image from ghcr.io then finally apply all the services and deployment yaml files

wait until all pods are up and running, then you can access our toktik at http://localhost:80/