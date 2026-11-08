# CD - Deploy Application from Jenkins Pipeline to EC2 Instance (automatically with docker)

<br>

## Technologies used:
AWS, Jenkins, Docker, Linux, Git, Java, Maven, Docker Hub

<br>

## Project Description:
- Prepare AWS EC2 Instance for deployment (Install Docker)
- Create ssh key credentials for EC2 server on Jenkins
- Extend the previous CI pipeline with deploy step to ssh into the remote EC2 instance and deploy newly built image from Jenkins server
- Configure security group on EC2 Instance to allow access to our web application

<br>

![Full EC2 pipeline](images/Full%20EC2%20pipeline.png)<br>
*Overview of the complete CI/CD pipeline I built: run tests, build the JAR, build the Docker image, push it to my private Docker Hub repo and deploy it to AWS EC2.*

<br>

![Jenkins complete EC2 pipeline](images/jenkins%20full%20complete%20ec2%20pipeline.png)<br>
*Successful run of my Jenkins multibranch pipeline, with all stages passing, including the new deploy stage that connects to the EC2 instance over SSH.*

<br>

![EC2 instance](images/EC2%20instance.png)<br>
*On the EC2 instance, `docker images` shows that the `simonknudsen7/demo-app:1.0` image was pulled from Docker Hub by the pipeline.*

<br>

![Result EC2 pipeline](images/result-ec2%20piepline.png)<br>
*The deployed application running in the browser on the EC2 instance's public IP, reachable after I opened the port in the security group.*
