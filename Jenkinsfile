pipeline {
    agent any
    parameters {
	    string(name: "branch", defaultValue: "master", description: "code branch")
      choice(name: 'module',choices: ['hotel-front','hotel-server'], description: 'module_name')
    }
    stages{
		stage('setting env') {
		  agent any
		  options {
			skipDefaultCheckout(true)
		  }
		  steps {
			script {
			  env.IMAGE_NAME = "registry.cn-hangzhou.aliyuncs.com/gaoyangtest/gaoyangtest/$module:build-$BUILD_NUMBER"
			}
		  }
		}	
        stage('get code'){
            steps{
			          git branch: '${branch}', credentialsId: '8a39c067-5817-4047-95c4-ba0b50024bd1', url: 'https://github.com/gaoyang02/angular-springboot-demo.git'
            }
        }
        stage('build') {
            parallel {
              stage('hotel-server-build') {
                when {
                  allOf{
                    environment name: 'branch', value: 'master'
                    environment name: 'module', value: 'hotel-server'
                  }
                }
                steps {
                  dir("hotel-server") {
                      sh "/usr/local/maven/bin/mvn -Dmaven.test.skip=true clean install"
                  }                  
                }
              }
              stage('hotel-front-build') {
                when {
                  allOf{
                    environment name: 'branch', value: 'master'
                    environment name: 'module', value: 'hotel-front'
                  }
                }
                steps {
                  dir("hotel-front") {
                      sh "/usr/local/node-v10.14.0-linux-x64/bin/npm config set registry https://mirrors.huaweicloud.com/repository/npm/"
                      sh "/usr/local/node-v10.14.0-linux-x64/bin/npm install"
                      sh "/usr/local/node-v10.14.0-linux-x64/bin/npm config set sass_binary_site https://mirrors.huaweicloud.com/node-sass/"
                      sh "/usr/local/node-v10.14.0-linux-x64/bin/npm install node-sass"
                      sh "/usr/local/node-v10.14.0-linux-x64/bin/npm run build"   
                  }                  
                }
              }
           }
        }                               
                        
        //stage('hotel-front image build'){
             //parallel {
               //stage('hotel-front-image-build') {
                 //when {
                   //allOf{
                   // environment name: 'branch', value: 'master'
                  //  environment name: 'module', value: 'hotel-front'
                  // }
               //  }
               //  steps {
               //    dir("hotel-front") {
               //         script {                          
				//		                 def imageName = docker.build("$IMAGE_NAME")
				//		                 docker.withRegistry('http://registry.cn-hangzhou.aliyuncs.com/gaoyangtest/gaoyangtest/','5dbc8045-871d-451b-874c-8756eda3dadc'){
				//		                  imageName.push()
				//		             }                                
                //         sh "/usr/bin/docker rmi ${IMAGE_NAME}"                               
				//		          }   
                //   }
                // }
               //}
                //stage('hotel-server-image-build') {
                 //when {
                   //allOf{
                    //environment name: 'branch', value: 'master'
                    //environment name: 'module', value: 'hotel-server'
                   //}
                 //}
                 //steps {
                   //dir("hotel-server") {
                        //script {                          
						  //               def imageName = docker.build("$IMAGE_NAME")
						  //               docker.withRegistry('http://registry.cn-hangzhou.aliyuncs.com/gaoyangtest/gaoyangtest/','5dbc8045-871d-451b-874c-8756eda3dadc'){
						  //                imageName.push()
						  //           }                                
                         //sh "/usr/bin/docker rmi ${IMAGE_NAME}"                               
						  //        }   
                   //}
                 //}
               //}
             //}
          //}

        //stage('hotel-front deploy k8s'){
        //  parallel {
        //    stage('hotel-front deploy') {
        //      when {
        //        allOf{
        //            environment name: 'branch', value: 'master'
        //            environment name: 'module', value: 'hotel-front'
        //        }
        //      }
        //      steps {
        //            script {
        //            sh "sed -i 's/<BUILD_TAG>/${BUILD_NUMBER}/g' hotel-front/k8s/k8s.yaml"
        //            sh "kubectl apply -f hotel-front/k8s/k8s.yaml --record"
        //          }
        //       }
        //    }
        //    stage('hotel-server deploy') {
        //      when {
        //        allOf{
         //           environment name: 'branch', value: 'master'
         //           environment name: 'module', value: 'hotel-server'
         //       }
         //     }
         //     steps {
         //           script {
         //           sh "sed -i 's/<BUILD_TAG>/${BUILD_NUMBER}/g' hotel-server/k8s/k8s.yaml"
         //           sh "kubectl apply -f hotel-server/k8s/k8s.yaml --record"
         //         }
         //      }
         //   }
         // }       
         //}    	            	  		   
    }       
    post {
        success {
            emailext (
                subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新正常",
                body: """
                详情：
                SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
                状态：${env.JOB_NAME} jenkins 更新运行正常 
                URL ：${env.BUILD_URL}
                项目名称 ：${env.JOB_NAME} 
                项目更新进度：${env.BUILD_NUMBER}
                代码分支：${env.brach}
                业务服务器IP: ${env.ip}
                发布模块: ${env.module}
                """,
                to: "zgui2000@163.com",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']]
                )
                }   
        failure {
            emailext (
                subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新失败",
                body: """
                详情：
                FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'             
                状态：${env.JOB_NAME} jenkins 运行失败 
                URL ：${env.BUILD_URL}
                项目名称 ：${env.JOB_NAME} 
                项目更新进度：${env.BUILD_NUMBER}
                代码分支：${env.brach}
                业务服务器IP: ${env.ip}
                发布模块: ${env.module}
                """,
                to: "zgui2000@163.com",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']]               
                )
                }
    
    }
}