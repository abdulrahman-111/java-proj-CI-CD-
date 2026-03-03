pipeline{
    agent{
            label "agent01"
    }
    tools{
        maven "maven-3-5-4"
        jdk "jdkfix-11"
    }

// 1st stage is to fetch code - but in our case it 
// is declaritive pipeline and jenkinsfile is on githb
// it will go and fetch the repo as all 

    stages{
         stage('Check Java') {
        steps {
                sh '''
                    echo JAVA_HOME=$JAVA_HOME
                    export PATH=$JAVA_HOME/bin:$PATH
                    which java
                    java -version
                '''
            }
        }
    
        stage("Build the app "){
            // sh allow us to run command 
            steps{
                
  sh '''
                    export PATH=$JAVA_HOME/bin:$PATH
                    mvn clean package -DskipTests
                '''        }
        }

        stage("test the app "){ 
            steps{
            sh "mvn  test"
        }
        }

        stage("Build the image "){
            steps{
            sh "docker build -t java-app:v1 ."
        }
        }

    }


}
