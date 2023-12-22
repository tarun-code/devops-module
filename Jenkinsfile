pipeline {
  agent any
  stages {
    stage('Software composition analysis') {
      steps {
        dependencyCheck(additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'OWASP-DC')
        dependencyCheckPublisher(pattern: 'dependency-check-report.xml')
        sh './dependency_check_report.sh'
      }
    }

  }
}