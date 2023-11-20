pipeline {
    agent any
    //always use double quotes for echo so that it works with accessing environmental variables

      tools {nodejs "Node"}

      options{
        timestamps() //Add timestamps to the Console Output
      }


    stages {
        stage('Workflow'){
            stages{
        stage('Clone'){
            steps{
                echo "Cloning..."
                git 'https://github.com/ramyabala221190/jenkinsTest.git'
            }
            post{
          success{
            echo 'Clone step completed'
          }
            }
        }
        
        stage('Install'){
            steps{
                echo 'Installing..'
                bat 'npm install'            
            }
             post{
          success{
            echo 'Install step completed'
          }
           }
        
        }
        stage("Build step") {
            // when{
            //     //execute the stage only when the env variable with key environment has value prod
            //     //we are providing value for this key when running the pipeline
            //     environment name: 'environment', value: 'prod'
            // }
            steps {
                echo "Building using ${environment} configuration"
                bat "npm run build:${environment}"

            }
            post{
              success{
            echo 'Build step completed'
              }
            }
        }
         
        // stage('Build using dev configuration'){
        //       when{
        //         //execute the stage only when the env variable with key environment has value dev
        //         //we are providing value for this key when running the pipeline
        //         environment name: 'environment', value: 'dev'
        //     }
        //     steps{
        //         echo "Building using ${environment} configuration"
        //         bat  'npm run build:dev'

        //     }
        //     post{
        //   success{
        //     echo 'Build(dev) step completed'
        //      }
        //      }
        // }
         
        stage('Test') {
            steps {
                echo 'Testing..'
            }
            post{
          success{
            echo 'Test step completed'
              }
             }
        }
         
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                fileOperations([fileCopyOperation(
                                  flattenFiles: true, //includes only files 
                                  includes: 'dist/*/**',
                                  targetLocation: "C:/Users/User/node/web/${environment}")]
                                  )

            }
             post{
          success{
            echo "Deploy step for #${env.BUILD_NUMBER} in ${environment} environment completed"
             }
            }
        
        }
         
      }
      post{
          failure{
            echo 'Workflow failed'
            //mail bcc: '', body: 'env.BUILD_TAG', cc: '', from: '', replyTo: '', subject: '# env.BUILD_ID Deployment for ${environment} failed.', to: 'ramya.bala221190@gmail.com'

          }
          success{
            echo 'Workflow succeeded'
            //mail bcc: '', body: 'env.BUILD_TAG', cc: '', from: '', replyTo: '', subject: '# env.BUILD_ID Deployment for ${environment} completed.', to: 'ramya.bala221190@gmail.com'
          }
         }
        }
    }
    
}