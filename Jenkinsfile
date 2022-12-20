pipeline {
    agent {
     label ("node1 || node2 ||  node3 || node4 ||  node5 ||  branch ||  main ||  jenkins-node || docker-agent ||  jenkins-docker2 ||  preproduction ||  production")
            }

options {
    buildDiscarder(logRotator(numToKeepStr: '2'))
    disableConcurrentBuilds()
    timeout(time: 1, unit: 'HOURS')
    timestamps()
    }  
  
    stages {

        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                        
                        choice(
                            choices: ['Dev', 'Sandbox', 'Prod'], 
                            name: 'Environment'
                                 
                                ),

                          string(
                            defaultValue: 's3user',
                            name: 'User',
			    description: 'Required to enter your name',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'DB-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'UI-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),

                            string(
                            defaultValue: 'eric-001',
                            name: 'Weather-Tag',
			    description: 'Required to enter your image tag',
                            trim: true
                            ),
                

                            string(
                            defaultValue: 'eric-001',
                            name: 'AUTH-Tag',
			    description: 'Required to enter your image tag',
                            trim: true

                            
                            ),
                        ])
                    ])
                }
            }
        }
        stage('Hello') {
            steps {
                sh '''
                ls 
                pwd
                lsblk
                uname -r
                echo 'everythings looking smooth!!!'
                '''
            }
        }
    }
}
