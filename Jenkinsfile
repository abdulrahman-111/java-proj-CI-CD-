pipeline{
    agent{
            label "agent01"
    }
    tools{
        maven "maven-3-5-4"
        jdk "jdk-11"
    }

// 1st stage is to fetch code - but in our case it 
// is declaritive pipeline and jenkinsfile is on githb
// it will go and fetch the repo as all 

    stages{
         stage('Check Java') {
        steps {
                sh '''
                    echo hi JAVA_HOME=$JAVA_HOME
                '''
            }
        }
    
stage('Build the app') {
    steps {
            sh 'java --version'    // just to verify
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
            sh "docker build -t java-app:v${BUILD_NUMBER} ."
        }
        }
        // stage("push the image "){
        //     steps{
        //   //  sh "docker login -u  -p " // using secret files 
        //    // sh "docker push docker.io/abdulrahman011/java-app:v${BUILD_NUMBER}"
        // }
        // }


    }


}
