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
       withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AKIA---------', credentialsId: 'AWS for HW10', secretKeyVariable: 'G--------']]) 
        {
            sh 'aws cloudformation describe-stack-resources --stack-name jenkins --region us-east-1' 
        }	     

    }
}
