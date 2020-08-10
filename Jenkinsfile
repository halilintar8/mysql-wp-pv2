pipeline{
    // 定义groovy脚本中使用的环境变量
    environment{        
    IMAGE_TAG4 =  sh(returnStdout: true,script: 'echo $image_tag4').trim()    
    ORIGIN_REPO =  sh(returnStdout: true,script: 'echo $origin_repo').trim()
    REPO =  sh(returnStdout: true,script: 'echo $repo').trim()
    BRANCH =  sh(returnStdout: true,script: 'echo $branch').trim()
    }

    agent{
        node{
            label 'slave-pipeline'
        }
    }

    stages{
        stage('Git'){
            steps{
            // git branch: '${BRANCH}', credentialsId: 'bintang_lenovo', url: 'git@bitbucket.org:m_bintang/jenkins-demo.git'
            git branch: '${BRANCH}', credentialsId: 'github_bintang', url: 'https://github.com/halilintar8/mysql-wp-pv2.git'
            }
        }

        // stage('Package'){
        //     steps{
        //         echo 'Running build automation'         
        //         container("npm-yarn") {                    
        //             sh "npm install"
        //             sh "npm i --package-lock-only"
        //             sh "npm audit fix"
        //             sh "yarn build"
        //         }
        //     }
        // }

        // stage('Docker Build Image') {
        //     steps{
        //         echo "building docker image"                
        //         container('docker') {
        //             // sh "docker ps -a"
        //             // sh "docker rmi 8eeadf3757f4 dfc29bfa7d41"
        //             // sh "docker images"
        //             sh "docker build -t ${ORIGIN_REPO}/${REPO} ."
        //             // echo "build docker image selesai"
        //         }
        //     }          
        // }

        // stage('Push Docker Image') {       
        //     steps {              
        //         container('docker') {
        //           echo "Push docker image to hub.docker.com"
        //             script {                      
        //               docker.withRegistry('', 'hub_docker_halilintar8') {
        //                 //app.push("${env.BUILD_NUMBER}")
                        
        //                 //app.push("${env.DOCKER_IMAGE_NAME}")
        //                 //app.push("latest")
                        
        //                 sh "docker tag ${ORIGIN_REPO}/${REPO} ${ORIGIN_REPO}/${REPO}:${IMAGE_TAG4}"
        //                 sh "docker push ${ORIGIN_REPO}/${REPO}:${IMAGE_TAG4}"


        //               }
        //             }
        //         }
        //     }
        // }
        
        stage('Deploy to Kubernetes') {
            steps {                
                container('kubectl') {
                    echo "Deploy to Kubernetes Cluster c"
                    script {
                        step([$class: 'KubernetesDeploy', authMethod: 'certs', apiServerUrl: 'https://10.8.1.120:6443', credentialsId:'k8sCertAuth', config: 'kustomization.yaml,mysql-deployment.yaml,wordpress-deployment.yaml', variableState: 'ORIGIN_REPO,REPO,IMAGE_TAG4'])
                        // step([$class: 'KubernetesDeploy', authMethod: 'certs', apiServerUrl: 'https://10.8.1.120:6443', credentialsId:'k8sCertAuth', config: './',variableState: 'ORIGIN_REPO,REPO,IMAGE_TAG4'])
                    }
                }
            }
            
        }
    }
}
