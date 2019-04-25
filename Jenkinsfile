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
      
        sh ("aws s3 cp /workspace/java-pipeline/dist/rectangle-${env.BUILD_NUMBER}.jar s3://lydia-hw10/ --recursive --exclude '*' --include '*.jar'")
    }
    
    stage('Reports'){
    
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                accessKeyVariable: 'AKIAJZ2I52TW2U6FOFIQ',
                secretKeyVaraible: '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']]) {
                               
                    sh "aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins"
                }
    }
}
