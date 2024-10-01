
Install Jenkins, java, docker
To integrate Jenkins with docker we jlnwed to install plug-ins
Manage jenkins-plugins-available plugins-search for docker 
Select docker, docker commons, docker pipeline docker api, docker build step and install with restart
Now go to dashboard
- - - as we gave installed docker plug-ins we need to integrate with Jenkins 
--manage jenkijs-tools--go to docker installation - add docker-give name-select install automatically - add installer - select download from docker.com-- Apply & save
Dashboard


-------------------------------------------------------------------------------------------------------
Over all pipeline script for reference:--

pipeline {
    agent any
    
    stages {
        stage ("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/Sharathpatel733/Netflix-Clone.git'
            }
        }
        stage("Build Docker image") {
            steps {
                sh "docker build -t netflix ."
            }
        }
        stage ("trivy Image Scan") {
            steps {
                sh "trivy image netflix:latest"
            }
        }
        stage ("pushing docker image to dockerhub") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh "docker tag netflix mrcloudtech/netflix:latest"
                        sh "docker push mrcloudtech/netflix:latest"
                    }
                }
            }
        }
        stage ("Run docker conytainer") {
            steps {
                sh "docker run -itd --name cont1 -p 4000:80 netflix:latest"
            }
        }
    }
}
---------------------------------------------------------------------------------

Docker file for reference

FROM ubuntu
RUN apt-get update -y
RUN apt-get install apache2 -y
COPY . /var/www/html/
EXPOSE 4000
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]

-------------------------------------------------------------------

https://github.com/Sharathpatel733/Netflix-Clone --Github link 
---------------------------------------------------------------------------------


Create new pipeline-new item-enter name docker select pipeline-ok and write the pipline

Git checkout 
 
Now open pipeline syntax in new window
In smaple step select git
Give git URL from the your github and update branch with main
Generate pipeline and copy the script and paste in the steps in the script
Build now
You can also check in server 
CD /var/lib/jenkins/workspace /
You can check check for docker container or images there should be no images 
Now we will write another stage 


stage:-2
Now will write Jenkins script to build the docker image 

Now you can check in the server there no images 
Before run give permissions
Chmod 777 /var/run/docker.sock
Once we run the build docker image will be created
FROM ubuntu
Run apt-get update - y
RUN apt-get install Apache 2 - y
Copy. /var/www/html/
EXPOSE 4000
CMD ["/usr/sbin/apachectl", "-D", FOREGROUND"] 
After build you can check image created in server 
We need to create a container from image 


docker image build
 

3rd stage:-

Now trivy will scan for any vulnerabilities
Now open docker hub


Trivy image scanner

 

Stage:4
Now let's push the docker image to docker hub  
Before that we need to give docker hub credentials to jenkins
Go to manage Jenkins-credentials - global-add crentials- give usename & password-Id give docker, give description docker and creare


Pushing docker image to docker hub

 
 

Take the script from the pipeline syntax
Select credentials, 
Stage:5
Generate pipeline script and paste in the pipeline script
And also right the script for pushing image to docker hub and create the container from image 


Run docker container

 
 

check in server 

 

Check the ip with docker port 4000

 
 
