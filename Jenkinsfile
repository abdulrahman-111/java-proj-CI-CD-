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
            
            sh """
                sed -i "s#.*image#        image: abdulrahman011/java-app:v${BUILD_NUMBER} "
                scp   deployment.yml jenkins@192.168.163.136:/home/jenkins/deployjenkins.yml 
                ssh jenkins@192.168.163.136   "kubectl apply -f /home/jenkins/deployjenkins.yml "

            """


        }
    }


}



}
