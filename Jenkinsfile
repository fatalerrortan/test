pipeline {
    agent any

    stages {
    
        stage('initialize parameters') {
            steps{
				script {

                    def remote = [:]
                    
                    stage("Connect to ${params['Host']} Via SSH"){
                        
                        remote.name = "${params['Host']}"
                        remote.host = "${params['Host']}"
                        remote.user = "${params['Username']}"
                        remote.password = "${params['Password_Server']}"
                        remote.allowAnyHosts = true  

                        sshCommand remote: remote, command: "hostname" 
                    }   
                        
                    stage('Retrieve Image from Docker Hub'){
 
                        /*params.each {param ->                     */
                        /*    echo "${param.key} and ${param.value}"*/
                        /*}                                         */

                        def deployment = "${params['Deployment']}"
                        def tag = "${params['Tag']}"
                        def cip_pw = "${params['Password_CIP']}"
                        
                        // Demo
            
                        sh "echo ${cip_pw} > tmp_pw.txt"
                        sshCommand remote: remote, command: "cat tmp_pw.txt | docker login -u ${params['Username_CIP']} --password-stdin" */
                        sshCommand remote: remote, command: "docker image ls -a" 
                        sshCommand remote: remote, command: "docker pull fatalerrortxl/tanmba_test:${tag}                            
                        sh "yes | rm tmp_pw.txt"

                        // backup
                        
                        // echo "${cip_pw} | docker login release.registry.cip.audi.de -u ${remote.user} --password-stdin"
                        // echo "docker pull release.registry.cip.audi.de/fabman/audi/${deployment}/fabman:${tag}"
                        // sh "echo ${cip_pw} > tmp_pw.txt"
                        // sshCommand remote: remote, command: "cat tmp_pw.txt | docker login release.registry.cip.audi.de -u ${remote.user} --password-stdin" 
                        // sshCommand remote: remote, command: "docker image ls -a"
                        // sshCommand remote: remote, command: "docker pull release.registry.cip.audi.de/fabman/audi/${deployment}/fabman:${tag}                            
                        // sh "yes | rm tmp_pw.txt"
                    
                    }

                    stage('Update docker-compose.yml Via Bitbucket'){

                        echo "todo update compose"
        
                    //     def repo_url = "https://${remote.user}:${cip_pw}@www.cip.audi.de/bitbucket/scm/fabman/docker_compose.git"

                    //     echo "[ ! -d '/home/username/fabman_cicd/' ] || git clone ${repo_url}"
                    //     echo "cd /home/username/fabman_cicd/ && git pull ${repo_url}"
                       
                    //    sshCommand remote: remote, command: "[ ! -d '/home/username/fabman_cicd/' ] || git clone ${repo_url}"     
                    //    sshCommand remote: remote, command: "cd /home/username/fabman_cicd/ && git pull ${repo_url}"              

                    }

                    stage('Deploy Image'){

                        echo "todo deploy image"

                        // // stop and remove all running containers 
                        // sshCommand remote: remote, command: "docker stop $(docker ps -aq) && docker rm $(docker ps -aq)"
                        // // execute docker-compose.yml 
                        // sshCommand remote: remote, command: "cd /home/username/fabman_cicd/ && docker-compose up"
                    }                                             
				}	
            }
        }
    }
}
