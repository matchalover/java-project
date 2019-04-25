properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    git url: 'https://github.com/matchalover/java-project.git', branch: 'master'
    
    stage('Unit Tests'){
       
        sh "ant -f test.xml -v"
        junit "reports/result.xml"
        
    }
    stage('Build'){
        git : 'https://github.com/matchalover/java-project.git'
        sh "ant -f build.xml -v"
        archiveArtifacts artifacts: "dist/*.jar"
    }
    
    stage ('Deploy') {
        sh "if ![ -d 'arn::aws:s3://lydia-hw10']; then 'aws mb arn::aws:s3://lydia-hw10'; fi"
        sh "aws s3 cp /workspace/java-pipeline/dist/*.jar s3://lydia-hw10/for-file-storage --exclude '*' --include '*.jar'"
    }
    
    stage('Reports'){
       withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AKIAS6JGZKMTEVNUPZF6', credentialsId: 'AWS keys name jenkins', secretKeyVariable: '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']]) 
        {
            sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1' 
        }	     

    }
}
