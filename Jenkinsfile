pipeline{
    agent any

    stages{
        stage ("git clone"){
            steps{
                git credentialsId: '4f383b42-16fd-447c-93db-38a46cdc1519', url: 'https://github.com/arshad41514/Java_Repo.git'
            }
        }

        stage ("build"){
            steps{
                sh 'mvn -f devops_jenkins/pom.xml clean install'
            }
        }

        stage ("test"){
            steps{
                echo "testing"
            }
        }

        stage ("publish"){
            steps{
                script{
                    try{
                        rtUpload (
                            serverId: "myjfrog"
                            spec: '''
                            {
                                "files": [
                                    "pattern": "target/*.war",
                                    "target": "myapp/"
                                ]
                            }
                            ''',
                            buildName: "${JOB_NAME}"
                            buildNumber: "${BUILD_NUMBER}"
                        )
                    }catch(error){
                        println "unable to push artifactory to jfrog"
                        pringln error.getMesage()
                    }
                }
            }
        }

        stage ("deploy"){
            steps{
                echo "ansible-playbook -i inventory e2e.yml"
            }
        }
 
    }

    post{
        success{
            build job: 'Project_1'
        }
    }
}