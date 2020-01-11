pipeline {
          agent {label 'docker'}
          parameters {
                      booleanParam(name: 'Stagin_Release ', defaultValue: true, description: 'Will push code on the Staging Environment')
                      booleanParam(name: 'Production_Release ', defaultValue: false, description: 'Will push code to the Production Environment')
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
                                        //sh ' cp /home/vikashkumar/Jenkins/workspace/gogo/package.json ./'
                                        //sh 'cp /home/vikashkumar/Jenkins/workspace/gogo/app.js ./'
                                        sh 'npm start &'
                            }
                                    
                  }
                  stage('Artifactory'){
                                        steps { 
                                                dir('Code') {
                                                                echo 'creating Artifacts'         
                                                                //archiveArtifacts 'target/*.war'
                                                                echo 'Artifact created'
                                                }
                                        }
                  }
                  stage('Approval Step'){
                                   when {
                                        expression { params.Stagin_Release == true}
                                   }
                                   steps{
                                           script {  
                                                            input message: 'Approve Deploy?', ok: 'Yes', submitter: 'Dev_xyz'
                                                }
                                   }
                  }
                  stage('Staging Release'){
                                        agent any
                                        when {
                                                expression { params.Stagin_Release == true }
                                        }
                                steps {
                                        echo 'Starting Release'
                                          // Here we need to provide the deployment path and credentails of either cloud / Inhouse server for eg: given below 
                                        //deploy adapters: [tomcat7(credentialsId: '51d79b50-5fd8-4444-91eb-c6ead7dc4151', path: '', url: 'http://abc.df.gh.xyz:8081')], contextPath: '/NodeAPP', war: 'Code/target/*.war'
                                        echo 'Release Completed'
                                }
                  }
                  stage('Approval Step'){
                                   when {
                                        expression { params.Production_Release == true}
                                   }
                                   steps{
                                           script {  
                                                            input message: 'Approve Deploy?', ok: 'Yes', submitter: 'PME_xyz'
                                                }
                                   }
                  }
                  stage('Production Release'){
                                        agent any
                                        when {
                                                expression { params.Production_Release == true }
                                        }
                                steps {
                                        echo 'Starting Release'
                                        // Here we need to provide the deployment path and credentails of either cloud / Inhouse server for eg: given below 
                                        //deploy adapters: [tomcat7(credentialsId: '51d79b50-5fd8-4444-91eb-c6ead7dc4151', path: '', url: 'http://abc.df.gh.xyz:8081')], contextPath: '/NodeAPP', war: 'Code/target/*.war'
                                        echo 'Release Completed'
                                }
                  }
          }
}
