# toktik-k8s
##  Project directory tree
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

The only repo you need to clone is `toktik-k8s` which contains all necessary deployment files and run the code below.

## Kubernetes Setup
Before running the following code make sure to install `helm` and `kubectl` 


To set up kubernetes, run (in toktik-k8s):
```bash
helm upgrade --install traefik traefik/traefik
kubectl create secret docker-registry ghcr-login-secret \
 --docker-server=https://ghcr.io \
 --docker-username={username} \ 
 --docker-password={personal_access_token}
kubectl apply -f .
```
The code above install trafik proxy using helm then create a k8s secret for pulling image from ghcr.io then finally apply all the services and deployment yaml files

wait until all pods are up and running, then you can access our toktik at http://localhost:80/
## Frontend
Our frontend (`toktik-frontend`) was built using React with ChakraUI as the primary component library. Some important files include the following
- `App.js` -- the 'entrypoint' into the frontend, so to speak.
- `HomePage.js` -- the homepage that displays videos
- `LoginPage.js`/`RegisterPage.js` -- the login/register pages
- `VideoPage.js` -- the page in which users view individual videos
- `UploadPage.js` -- the page in which users upload videos
- `UserContext.js` -- handles authentication from backend

## Backend
Our backend (`toktik-backend` was implemented using FastAPI. 
- For endpoints/functionality, in `routers/`, there are the following files
	- `processing.py` -- related to communication with services / the video processing sstage
	- `s3.py` -- methods for interacting with S3 (i.e., presigned URL)
	- `users.py` -- user creation, management, and authentication stuff
	- `videos.py` -- related to video stuff (in DB) and CloudFront signed URL generation
- For database stuff
	- `schemas.py` and `models.py` contain the schema for each table in our database
	- `db_services.py` contains all methods which involve interaction with our database.
## Workers
Video encoding, chunking, and thumbnail generation services can be found the the `toktik-encode-worker`, `toktik-chunk-worker`, and `toktik-thumbnail-worker`, respectively. Each worker contains a Python file with the core functionality (the structure is pretty much the same for all).
## Schedulers
We have two schedulers since our workers are "backend-phobic", so to speak. It has been pointed out by Ajarn Weerapong that we can remove these and have the backend/workers communicate directly, however, and we will make these changes for Project 3.
