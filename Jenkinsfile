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
        DEPi ="round 4 "
    }

// 1st stage is to fetch code - but in our case it 
// is declaritive pipeline and jenkinsfile is on githb
// it will go and fetch the repo as all 

    stages{
    
    stage('Build the app') {
        steps {
            sh "echo ${DEPI}" 
            sh 'mvn clean package -DskipTests'
        }
    }

    stage("test the app "){ 
        steps{
            sh "mvn  test"
        }
    }

    stage("Build the image "){
        steps{
            sh "docker build -t ${DOCKER_USERNAME}/java-app:v${BUILD_NUMBER}   ."
        }
    }
    stage("push the image "){
        steps{
            // withCredentials([string(credentialsId: 'DOCKER_PASS', variable: 'DOCKER_PASS'), string(credentialsId: 'DOCKER_USERNAME', variable: 'DOCKER_USERNAME')]) {
            //     sh "docker login -u $DOCKER_USERNAME -p  $DOCKER_PASS " // using secret files 
            //     sh "docker push docker.io/${DOCKER_USERNAME}/java-app:v${BUILD_NUMBER}"

            // }

            sh "docker login -u $DOCKER_USERNAME -p  $DOCKER_PASS " // using secret files 
            sh "docker push abdulrahman011/java-app:v${BUILD_NUMBER}"

        }
    }




    }


}
