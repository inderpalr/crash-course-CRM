pipeline {
    agent any
    stages {
        stage('DeployToStaging') {
            //when {
          //      branch 'master'
          //  }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        //sourceFiles: 'dist/trainSchedule.zip',
                                        //removePrefix: 'dist/',
                                        //remoteDirectory: '/tmp',
                                        execCommand: 'cd /home/Documents && sudo apt-get update && sudo apt-get upgrade && sudo apt install python3-pip && sudo pip3 install virtualenv && virtualenv crm1 && cd crm1/bin/ && chmod +x activate && source activate && cd .. && sudo apt-get install python3-dev && git clone -b Part-20-Password-Reset-Email https://github.com/inderpalr/crash-course-CRM.git && cd crash-course-CRM && pip3 install -r requirements.txt && cd crm1 && python3 manage.py runserver 0.0.0.0:8080 '
                                     //   execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                input 'Does the staging environment look OK?'
                milestone(1)
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'production',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        //sourceFiles: 'dist/trainSchedule.zip',
                                        //removePrefix: 'dist/',
                                        //remoteDirectory: '/tmp',
                                        execCommand: 'cd /home/Documents && sudo apt-get update && sudo apt-get upgrade && sudo apt install python3-pip && sudo pip3 install virtualenv && virtualenv crm1 && cd crm1/bin/ && chmod +x activate && source activate && cd .. && sudo apt-get install python3-dev && git clone -b Part-20-Password-Reset-Email https://github.com/inderpalr/crash-course-CRM.git && cd crash-course-CRM && pip3 install -r requirements.txt && cd crm1 && python3 manage.py runserver 0.0.0.0:8080'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
