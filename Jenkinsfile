properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    git url: 'https://github.com/UST-SEIS665/hw10-seis665-01-spring2019-matchalover.git', branch: 'master'
    
    stage('Unit Tests'){
       
        sh "ant -f test.xml -v"
        junit "reports/result.xml"
        
    }
    stage('Build'){
        git 'https://github.com/matchalover/java-project.git'
        sh "ant -f build.xml -v"
    }
    
    stage ('Deploy') {
      steps {
        sh "if ![ -d 'arn::aws:s3://lydia-hw10'], then 'aws mb arn::aws:s3://lydia-hw10'; fi"
        sh ("aws s3 cp $WORKSPACE/target/rectangle-${env.BUILD_NUMBER}.jar s3://lydia-hw10/${env.BRANCH_NAME}/ --recursive --exclude '*' --include '*.jar'")
      }
    }
    
    stage('Reports'){
        steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                accessKeyVariable: 'AKIAJZ2I52TW2U6FOFIQ',
                              secretKeyVaraible: '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']]) {
                               
                    sh "aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins"
            }
        }
    }
}
