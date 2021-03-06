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
                        remote.user = "${params['Username_Server']}"
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
            
                        sh "echo ${cip_pw} > tmp_pw.txt"
                        sh "cat tmp_pw.txt"
                        sshCommand remote: remote, command: "cat tmp_pw.txt | docker login -u ${params['Username_CIP']} --password-stdin" 
                        sshCommand remote: remote, command: "docker image ls -a" 
                        sshCommand remote: remote, command: "docker pull httpd:${Fabman_Tag}"    
                        sshCommand remote: remote, command: "docker pull redis:${WebUI_Tag}"                         
                        sh "yes | rm tmp_pw.txt"
                    
                    }

                    stage('Update docker-compose.yml Via Bitbucket'){

                       
        
                    // def repo_url = "https://${remote.user}:${cip_pw}@www.cip.audi.de/bitbucket/scm/fabman/docker_compose.git"

                    //     echo "[ ! -d '/home/username/fabman_cicd/' ] || git clone ${repo_url}"
                    //     echo "cd /home/username/fabman_cicd/ && git pull ${repo_url}"
                        def repo_url = "https://github.com/fatalerrortan/test.git"  
                        sshCommand remote: remote, command: "ls -al"
                        sshCommand remote: remote, command: "[ -d 'test' ] || git clone ${repo_url}"     
                        sshCommand remote: remote, command: "cd test && git pull origin ${Docker_Compose_Branch}"              
                        sh "ls -al"
                    }

                    stage('Deploy Image'){
                        try{
                            // stop and remove all running containers 
                            sshCommand remote: remote, command: "docker stop \$(docker ps -aq) && docker rm \$(docker ps -aq)"
                        }catch(err){
                            // execute docker-compose.yml 
                            sshCommand remote: remote, command: "cd test && docker-compose up -d"
                        }
                        sshCommand remote: remote, command: "cd test && docker-compose up -d"
                    }                                             
				}	
            }
        }
    }
}
