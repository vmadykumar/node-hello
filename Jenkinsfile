pipeline {
          agent {label 'ubuntu'}
          parameters {
                      booleanParam(name: 'Push code to Staging Environment', defaultValue: true, description: 'Will push code on the Staging Environment')
                      booleanParam(name: 'Push code to Production Environment', defaultValue: false, description: 'Will push code to the Production Environment')
          }
          stages{
                  stage('checkout') {
                                      echo 'Checking out code from GitHub'
                                      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/vmadykumar/node-hello.git']]])
                                    
                  }
                  stage ('build') {
                                    echo 'building source code'
                                    sh 'node start'
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
