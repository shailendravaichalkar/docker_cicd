def remote = [:]
remote.name = "node"
remote.host = "devopslinux"
remote.allowAnyHosts = true

node {
    withCredentials([usernamePassword(credentialsId: 'a72759b4-2f4f-4545-b1f7-db1fa4e6f977', passwordVariable: 'password', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.password = password
        stage("BUILD") {
            sshCommand remote: remote, command: "/home/devops/docker/update_docker.sh shailendravaichalkar/html-demo ${BUILD_ID}" 
        }
        stage("Deploy") {
            sshCommand remote: remote, command: "/home/devops/docker/deploy_docker.sh shailendravaichalkar/html-demo"
        }
        stage("Post") {
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                 subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                 to: "shailendra.v@fiserv.com"
            echo "Mail Sent Successful"
        }
    }
}