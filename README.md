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