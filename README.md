# What is Node.js?
-> Node.js is a javascript runtime environment which is:
    Open Source and Cross-Platform

-> Used to execute javascript code server side instead of client side.
-> Has an event-driven architecture capable of asynchronous I/O.
-> This makes NodeJS capable of responding very fast to requests.
-> It can immediately give a client the result and asynchronously handle database updates which generally take a longer time.
-> This is one of the reasons why NodeJS is used so much.
-> We use Node.js example project because it's relatively easy to understand, even when you never have used it before.
-> It doesn't need to be compiled, so building the app doesn't take a lot of memory (unlike building java projects).
-> Which is handy if you ust want to do this course on small instances.
-> Once you understand how to build a simple node.js project, you can use the knowledge to start building larger, more complex projects.

# How to build a Node.js app?

01. Install dependencies (npm install).
-> Downloads and installs all the dependencies.

02. Run tests (npm test).
-> Runs all the Node.js tests.
-> Typically if a test fails the build fails and the developer should be notified.
-> The only thing missing is a package: how do you package and distribute this project so it can be deployed on a server instance.

03. Package the app in docker
-> You can create a container that includes the Node.js binaries and your project.
-> Rather than creating a .zip, .jar, or .tgz package, you package a binary that includes all binaries and dependencies.
-> That way we are sure that the code will behave the same on the production system as your dev/test/staging/qa machines.

-> It gives closer dev-prod parity.
04. Distribute the docker image.
-> We can use a docker registry to upload our docker images.
-> Those images can be set public (everyone can access them).
-> Example: Docker Hub, Amazon AWS ECR etc.

# Install necessary plugins in Jenkins.
-> NodeJS plugin
-> CloudBees Docker Build and Publish plugin

01. Create new job to build without docker.
-> Manage Jenkins > Global Tool Conf > Add NodeJS > "nodejs"
-> Create New Job > "node-js-app" > Freestyle project > OK
-> Choose Git and paste git https url.
-> On Build Environment > Provide Node & npm bin/ folder to PATH
-> On build sections select Execute Shell > "npm install" > Save
-> BUILD NOW
To check the result ssh to aws jenkins instance > cd /var/jenkins_home/workspace/ > ls node-js-app

02. Create new job to build NodeJS app in the docker image and push the image to DockerHub repository.

-> SSH to aws jenkins instance > 
git clone https://github.com/MrzvUz/jenkins-docker.git > cd jenkins-docker > docker build -t jenkins-docker .

-> docker ps > Stop and remove the running jenkins container -> docker stop jenkins > docker rm jenkins > ls /var/jenkins_home/ to check the remaining files.
-> Run newly created docker images with docker client socket to connect and communicate with running aws instance.

-> docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins -d jenkins-docker

-> docker ps -a to see the running container on aws instance.

-> ls -ahl /var/run/docker.sock to see docker.sock owned by root and group with name docker.

-> Test it before testing in Jenkins by typing: docker exec -it jenkins bash > and inside the container we can run docker ps -a to check docker running inside the jenkins container which is communicating with docker socket running on aws jenkins instance.

-> Go back to Jenkins node-js-app > Configure > Build > Docker Build and Publish > give DockerHub repository mrzvuz/docker-nodejs-app > Registry Credentials > username and password > Apply and Save > Build Now.

-> To test the result ssh to aws instance and pull just created docker-nodejs-app image by typing: 
$~ docker pull mrzvuz/docker-nodejs-app
$~ docker run -p 3000:3000 -d --name nodejs-app mrzvuz/docker-nodejs-app
$~ docker ps
$~ curl localhost:3000
