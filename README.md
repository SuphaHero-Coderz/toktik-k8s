# toktik-k8s
before running the following code make sure to install helm and kubectl 

don't forget to add `.env` file in this repo before setting up

To set up kubernetes, run:
```bash
helm upgrade --install traefik traefik/traefik
kubectl create secret docker-registry ghcr-login-secret \
 --docker-server=https://ghcr.io \
 --docker-username={username} \ 
 --docker-password={personal_access_token}I
kubectl apply -f .
```
The code above install trafik proxy using helm then create a k8s secret for pulling image from ghcr.io then finally apply all the services and deplyments yaml