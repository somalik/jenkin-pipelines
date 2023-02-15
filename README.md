# jenkin-pipelines


### Remote Directory copy

stage("build deploy-dev"){
         steps{
            dir('/var/lib/jenkins/workspace/docagent/publish') {
                script{
               sshPublisher(
                   continueOnError: false, failOnError: true,
                     publishers: [
                        sshPublisherDesc(
                            configName: 'dev6', 
                            transfers: [
                            sshTransfer(cleanRemote: true,
                            excludes: '',
                           // execCommand: 'pwd',
                            execTimeout: 120000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: '/data/application/',
                            remoteDirectorySDF: false,
                            removePrefix: '',
                            sourceFiles: '*.zip')],
                        //    sourceFiles: '*')],
                            usePromotionTimestamp: false,
                            useWorkspaceInPromotion: false,
                            verbose: true)]) 
                        }
                   }
                }
             } 

 ###    AWS S3 UPload

 
            stage('upload to AWS S3'){
            steps{
                dir('/data/build'){
                      script{
                          s3Upload consoleLogLevel: 'INFO',
                          dontSetBuildResultOnFailure: false,
                          dontWaitForConcurrentBuildCompletion: false, 
                          entries: [[bucket: 'client-artifacts',
                                              excludedFile: '', 
                                              flatten: false, 
                                              gzipFiles: false, 
                                              keepForever: false, 
                                              managedArtifacts: true,  
                                              noUploadOnFailure: true,  
                                              selectedRegion: 'ap-northeast-1',
                                              showDirectlyInBrowser: true,
                                              sourceFile: '*.zip', 
                                              storageClass: 'STANDARD', 
                                              uploadFromSlave: false, 
                                              useServerSideEncryption: false]],
                                              pluginFailureResultConstraint: 'FAILURE',
                                              profileName: 'helloaws',
                                              userMetadata: []
                     }
                }
          }
     }

             



 ### get the Git committer email address.


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



 ### get the Git commit TAG

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
 
 ### TAG NAME and Version Number Split and make variable and print
 
 
def serviceHashmap = [SVC1:false,SVC2:false,SVC3:false]
def versionHashmap = [SVC1_Version:"V0",SVC2_Version:"V0",SVC3_Version:"V0"]
tag1 = 'DEP_MAIN_SVC1-V1+SVC2-V1';
def array = tag1.split('_')
//def array = tag_name.split('_')
def serviceNeedToDeploy=array[2].split('\\+')
def SVC1=false,SVC2=false,SVC3=false
def SVC1_Version='V0',SVC2_Version='V0',SVC3_Version='V0'

for(service in serviceNeedToDeploy){
    sArray=service.split('-');
    if (sArray[0] == "SVC1") {SVC1=true
    SVC1_Version = sArray[1]    
    }else if (sArray[0] == "SVC2") {SVC2=true
    SVC2_Version = sArray[1]    
    }else if (sArray[0] == "SVC3") {SVC3=true
    SVC3_Version = sArray[1]    
    }
    serviceHashmap["${sArray[0]}"]=true
    versionHashmap["${sArray[0]}_Version"]=sArray[1]
    
    
}

println(serviceHashmap)
println(versionHashmap)
println("SVC1= "+SVC1+" Version = "+SVC1_Version)
println("SVC2= "+SVC2+" Version = "+SVC2_Version)
println("SVC3= "+SVC3+" Version = "+SVC3_Version)


