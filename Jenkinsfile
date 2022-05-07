// Build a Maven project using the standard image and Scripted syntax.
// Rather than inline YAML, you could use: yaml: readTrusted('jenkins-pod.yaml')
// Or, to avoid YAML: containers: [containerTemplate(name: 'maven', image: 'maven:3.6.3-jdk-8', command: 'sleep', args: 'infinity')]
pipeline{
    agent{
        kubernetes{
            defaultContainer 'builder'
            yaml  '''
apiVersion: v1
kind: Pod
spec:
containers:
- name: builder
    image: maven:3.6.3-jdk-8
    command:
    - sleep
    args:
    - infinity
            '''
        }
    }
    stages{
        stage('Testing'){
            steps{
                sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
                junit 'target/surefire-reports/TEST-*.xml'
            }
        }
        stage('Building'){
            steps{
                sh 'mvn install'
                archiveArtifacts 'target/*.jar'
            }
        }
    }
}
