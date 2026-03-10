@Library("DEPI")_



pipeline{
    agent{
            label "agent01"
    }
    tools{
        maven "maven-3-5-4"
        jdk "jdk-11"
    }
    environment {
        DOCKER_USERNAME = credentials("DOCKER_USERNAME")
        DOCKER_PASS = credentials("DOCKER_PASS")
    }


    stages{
    
        stage('Build the app') {
            steps {
            script{
                    def maven_func = new io.depi.maven() // create objec of class
                    maven_func.javaBuild("-DskipTests")
            }
            }
        }

        stage("test the app "){ 
            steps{
            script{
                    def maven_func = new io.depi.maven() // create objec of class
                    maven_func.javatest("")
            }
        }
        }

        stage("Build the image "){
            steps{
            script{
                    def dcoker_func = new io.depi.docker() // create objec of class
                    dcoker_func.build("abdulrahman011/java-app","v${BUILD_NUMBER}")
            }
        }
        }
        stage("push the image "){
            
                //  withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PASS'), string(credentialsId: 'DOCKER_USERNAME', variable: 'DOCKER_USERNAME')]) {
                //     sh "docker login -u $DOCKER_USERNAME -p  $DOCKER_PASS " // using secret files 
                //     sh "docker push docker.io/${DOCKER_USERNAME}/java-app:v${BUILD_NUMBER}"

                // }
        steps{
                script{
                    def dcoker_func = new io.depi.docker() // create objec of class
                    dcoker_func.login_docker("$DOCKER_USERNAME","$DOCKER_PASS")
                    dcoker_func.push("abdulrahman011/java-app","v${BUILD_NUMBER}")
            }    
        }
        }
        stage("deploy with k8s"){
            steps{
                //sh "cd java-cd"
                // we use the plugin as if the repo already cloned -> we need to pull it 
                // git branch: 'main', url: 'git@github.com:abdulrahman-111/java-cd.git'
                // install all the content not as dir for the repo and replace all files in the place of CI 
                // so we create dir and cd to it  before git plugin -> so our CD files in separate Dir 
                sh """
                    // this will handle 
                    if [ -d "java-cd" ]; then cd java-cd && git pull ; else git clone it@github.com:abdulrahman-111/java-cd.git && cd java-cd ; fi 

                    cd java-UI
                    sed -i "s#.*image: .*#        image: abdulrahman011/java-app:v${BUILD_NUMBER}#g" deployment.yml
                    git config user.email "abdulrahman@gmail.com"
                    git config user.name "boda"
                    git add .
                    git commit -m "change imaege to verison ${BUILD_NUMBER} by jenkins"
                    git push 
                """
                
            }
        }
    }
}
