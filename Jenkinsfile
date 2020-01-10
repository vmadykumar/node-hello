pipeline {
          agent {label 'docker'}
          parameters {
                      booleanParam(name: 'Push code to Staging Environment', defaultValue: true, description: 'Will push code on the Staging Environment')
                      booleanParam(name: 'Push code to Production Environment', defaultValue: false, description: 'Will push code to the Production Environment')
          }
          stages{
                  stage('checkout') {
                            steps{
                                  echo 'Checking out code from GitHub'
                                  checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/vmadykumar/node-hello.git']]])                            
                                  echo 'checkout completed'
                            }
                                                                          
                  }
                  stage ('build') {
                            steps{
                                       
                                        echo 'building source code'
                                        //sh 'npm install'
                                        sh 'nohup npm start'
                            }
                                    
                  }
                  stage('Artifactory'){
                                        steps { 
                                                dir('Code') {
                                                                echo 'creating Artifacts'         
                                                                archiveArtifacts 'target/*.war'
                                                                echo 'Artifact created'
                                                }
                                        }
                  }
          }
}
