# jenkin-pipelines
### get the Git committer email address
_______________________________________
stage('Run Required Scripts') {
        steps {
           script {
                GIT_COMMIT_EMAIL = sh (
                script: 'git --no-pager show -s --format=\'%ae\'',
                returnStdout: true
                ).trim()
    echo "Git committer email: ${GIT_COMMIT_EMAIL}"
}
            }
       }
 ######################################################################
 ### get the Git commit TAG
_______________________________________

     stage('Run Required Scripts') {
        steps {
           script {
                tag_name = sh (
                script: 'git describe --tags `git rev-list --tags --max-count=1`',
                returnStdout: true
                ).trim()
    echo "Git tag is : ${tag_name}"
}
            }
       } 
 
 
