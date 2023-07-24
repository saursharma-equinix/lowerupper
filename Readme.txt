#Flow of pipeline..

1. fetch git code, so you have to replace you git repo in that stage with branch..if you have secured
git repo than also have to provide credentials for that..

2. This is java application deployment, so we perform mvn build here.

3. build docker image..

4. sonar test analysis, you have to provide you sonar url in this step..

5. docker image push to nexus repo
before this step perform you host where this deloyment is running or where from you want to push your 
application docker image to nexus repo, docker must be access your nexus repo
if your repo is running on http then you have to entry in /etc/docker/daemon.json file as 

"{ "insecure-registries": ["NexusRepoURL:NexusPort"] }"

then restart your docker service..also ensure that your docker command runs without sudo, for that you
have to add your user in docker group with command : 

"usermod -aG docker $USER"

then once logout and login your session then you will able to run docker as non sudo user..

6. in this step we deploy our application in kubernetes pod, so in kubernetes pod environment your Nexus
repo must be accessible to pull that docker images from repo.. for that we have to add a secret on kubernetes
and provide context of kubernetes config file on jenkins host so that we can deploy our application..

to add screat on kubernetes host do as: 
"kubectl create secret docker-registry secret_name \
  --docker-server=<your-nexus-registry-url> \
  --docker-username=<your-username> \
  --docker-password=<your-password> "
