pipeline{
    agent {label "slave123"}

    stages{
        stage ("git clone"){
            steps{
                sh 'mkdir -p applogin'
                dir("applogin"){
                git credentialsId: '4f383b42-16fd-447c-93db-38a46cdc1519', url: 'https://github.com/arshad41514/applogin.git'
                }
                
                sh 'mkdir -p ansible'
                dir("ansible"){
                    git credentialsId: '4f383b42-16fd-447c-93db-38a46cdc1519', url: 'https://github.com/arshad41514/ansible_workflow.git'
                }   
            }
        }

        stage ("build"){
            steps{
                sh 'mvn -f applogin/pom.xml clean install'
            }
        }

        stage ("test"){
            steps{
                echo 'testing'
            }
        }
        
        /*stage ("publish") {
            steps {
                sh 'cd java_e2e'
                script {
                    try {
                        rtUpload ( 
                            serverId: 'serverid_jfrog',
                            spec: '''
                                {
                                    "files": [
                                        {
                                            "pattern": "target/*.jar",
                                            "target": "java_e2e/"
                                        }
                                    ]
                                }
                            ''',
                            buildName: "${JOB_NAME}",
                            buildNumber: "${BUILD_NUMBER}"
                        )
                    }
                    catch(err) {
                        println "unable to push the artifact kindly check the configuration"
                        println err.getMessage()
                    }
                }
                
            }
        }*/
        
        stage ("deploy"){
            steps{
                sh 'ansible-playbook -i tom_hosts tom.yml'
            }
        }        
    }

    /*post{
        always{
            emailext body: '''This is the build status

            job: "${JOB_NAME}"
            url: "${BUILD_URL}"''', subject: 'Build status', to: 'arshad.mk1612@gmail.com'
        }

        success{
            build job: 'Project_1'
        }
    }*/
}
