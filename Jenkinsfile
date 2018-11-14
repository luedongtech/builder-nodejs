pipeline {
    agent {
        label "jenkins-jx-base"
    }
    environment {
        ORG         = 'luedongtech'
        APP_NAME    = 'builder-nodejs'
        REGISTRY	= 'registry-vpc.cn-hangzhou.aliyuncs.com'
    }
    stages {
        stage('CI Build and push snapshot') {
            when {
                branch 'PR-*'
            }
            steps {
                container('jx-base') {
                    sh "docker build -t $REGISTRY/$ORG/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER ."
                    sh "docker push $REGISTRY/$ORG/$APP_NAME:SNAPSHOT-$BRANCH_NAME-$BUILD_NUMBER"
                }
            }
        }
    
        stage('Build and Push Release') {
            when {
                branch 'master'
            }
            steps {
                container('jx-base') {
                    sh "jx step git credentials"
                    sh "./jx/scripts/release.sh"
                }
            }
        }
    }
}
