# C3-Project

  => This project contains a simple node application that prints "Hello World" text using Docker orchestration.

  => The Dockerfile uses Node-16-alpine image for less storage space.

  => Jenkinsfile has the procedure to clone this repository into the target machine, build the docker image from Dockerfile, push the docker image to the Elastic Container Registry, run the docker image in the App Host(worker1 node).

  => The Node app runs on port 8080 and the port has to be configured in the Target group of the ALB by forwarding the incoming requests with the path "/app*" to the App host.

  => The Jenkins host runs on port 8080 by default and the port has to be configured in the Target group of the ALB by forwarding the incoming requests with the path "/jenkins*" to the Jenkins host.
