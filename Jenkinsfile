pipeline {

      environment {
        imagename = 'imagejenkinstp6'
        registryCredential = 'docker'
      }

      agent any
      stages {
        stage('Clone sources') {
            steps {
                git url: 'https://github.com/Bastien/TP6_formation.git',
                branch: "main"
            }
        }
        stage('continuous integration') { // Compile and do unit testing
             tools {
               gradle 'gradle'
            }
             steps {
                 parallel (
                 // run Gradle to execute compile and unit testing
                    pylint: {
                        sh 'gradle lint'
                    },
                    pycode: {
                        sh 'gradle pycode'
                    }
                )
             }
           }

        // stage('testcode') {
             // tools {
             //   gradle 'radle'
            // }
        //     steps {
        //         sh 'gradle test'
        //     }
        // }

        stage('Package and deploy') {
             tools {
               gradle 'gradle'
             }
            steps {
                sh 'gradle up'
            }
        }

        stage('Build docker image') {
            steps{
                script {
                    docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }


        //stage('Push Image') {
        //    steps{
        //        script {
         //           docker.withRegistry( '', registryCredential ) {
         //               dockerImage.push("$BUILD_NUMBER")
          //          }
        //        }
         //   }
        //}

        //stage('Test image docker') {
        //    steps {
        //        sh 'docker run -d --name cont-jenkinstp6:cont-jenkinstp6 -p 3002:3002 --rm formation2/cont-jenkinstp6:cont-jenkinstp6'
         //   }
        //}
    }
 }
