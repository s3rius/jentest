podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: maven
        image: maven:3.8.1-jdk-8
        command:
        - sleep
        args:
        - 99d
''') {
  node(POD_LABEL) {
    stage('JenTest Project') {
      git 'https://github.com/s3rius/jentest.git'
      container('maven') {
        stage('Test a maven project') {
            sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
            junit 'target/surefire-reports/TEST-*.xml'
        }
        stage('Build a project'){
            sh 'mvn install'
            archiveArtifacts 'target/*.jar'
        }
      }
    }
  }
}

