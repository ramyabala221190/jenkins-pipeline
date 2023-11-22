/* groovylint-disable NestedBlockDepth */
/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    //always use double quotes for echo so that it works with accessing environmental variables
    //dont use variables in stage names. error will be thrown

    tools { nodejs 'Node' }

    options {
        timestamps() //Add timestamps to the Console Output
    }


    stages {
        stage('Workflow') {
            stages {
                stage('Clone') {
                    steps {
                        echo 'Cloning...'
                        cleanWs() //Workspace Clean Plugin to delete the workspace before cloning and building
                        git 'https://github.com/ramyabala221190/jenkinsTest.git'
                    }
                    post {
                        success {
                            echo 'Clone step completed'
                        }
                    }
                }

                stage('Install') {
                    steps {
                        echo 'Installing..'
                        bat 'npm install'
                    }
                    post {
                        success {
                            echo 'Install step completed'
                        }
                    }
                }
                stage('Build step')  {
                    // when{
                    //     //execute the stage only when the env variable with key environment has value prod
                    //     //we are providing value for this key when running the pipeline
                    //     environment name: 'environment', value: 'prod'
                    // }
                    steps {
                        echo "Building using ${environment} configuration"
                        bat "npm run build:${environment}"
                    }
                    post {
                        success {
                            echo 'Build step completed'
                        }
                    }
                }


                stage('Test') {
                    steps {
                        echo 'Testing..'
                        bat 'npm run test'
                    }
                    post {
                        success {
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
                                  targetLocation: "C:/Users/User/nginx/nginx-1.25.3/html/${environment}")]
                                  )

                    }
                    post {
                        success {
                            echo "Deploy step for #${env.BUILD_NUMBER} in ${environment} environment completed"
                        }
                    }
                }
            }
            post {
                failure {
                    echo 'Workflow failed'
                }
                success {
                    echo 'Workflow succeeded'
                }
            }
        }
    }
}