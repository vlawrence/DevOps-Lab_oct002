pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "Continuous Integration on branch ${env.BRANCH_NAME}"
        echo 'Build libraries'
        echo 'Test deployment'
        echo 'Run unittests'
      }
    }
    stage('Deploy') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        echo 'Pack Images'
      }
    }
    stage('Regression') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      parallel {
        stage('Business') {
          steps {
            echo 'Check business functionality'
          }
        }
        stage('Integration') {
          steps {
            echo 'Check interfaces/integrations'
          }
        }
        stage('Performance') {
          steps {
            echo 'Check performance on load'
          }
        }
        stage('Stability') {
          steps {
            echo 'Run chaos monkeys'
          }
        }
      }
    }
    stage('Acceptance') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        input 'Accept for production?'
      }
    }
    stage('Production') {
      when { not { expression { BRANCH_NAME ==~ /^feature.*/ } } }
      steps {
        echo 'Production deployment'
      }
    }
  }
}
