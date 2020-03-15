node {
    
    stage ('Scm Checkout'){
        git 'https://github.com/mtajic/StudentSurvey.git'
        
    }
    
    stage ('Mvn Package'){
        
        //def mvn = "${mvnHome}/bin/mvn"
        //sh "${mvn} clean package"
        sh 'mvn clean package'
    }
    
    stage ('Build Docker Image'){
        sh 'docker build -t mtajic/swe645:studentsurvey01 .'
    }
    
    stage ('Push Docker Image'){
        withCredentials([string(credentialsId: 'Dockerp', variable: 'Dockerp')]) {
            sh "docker login -u mtajic -p ${Dockerp}"
        }
        sh 'docker push mtajic/swe645:studentsurvey01'
    }
    
    stage ('Run Container on Kubernetes'){
        sshPublisher(publishers: [sshPublisherDesc(configName: 'ubuntu@18.211.123.192', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo kubectl rollout restart deployment/kubernetes-swe645', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
    } 
    
}