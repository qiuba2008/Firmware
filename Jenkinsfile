pipeline {
  agent {
    docker {
      image 'px4io/px4-dev-simulation:2017-09-26'
      args '--env=CCACHE_DISABLE=1'
    }
  }
  stages {
    stage('Build') {
      parallel {
        stage('nuttx_px4fmu-v2_default') {
          steps {
            sh 'make px4fmu-v2_default'
          }
        }
        stage('posix_sitl_default') {
          steps {
            sh 'make posix_sitl_default'
          }
        }
        stage('nuttx_px4fmu-v3_default') {
          steps {
            sh 'make px4fmu-v3_default'
          }
        }
      }
    }
    stage('Test') {
      parallel {
        stage('tests') {
          steps {
            sh 'make tests'
          }
        }
        stage('check_format') {
          steps {
            sh 'make check_format'
          }
        }
        stage('clang-tidy') {
          steps {
            sh 'make clang-tidy-quiet'
          }
        }
        stage('tests_coverage') {
          steps {
            sh 'make tests_coverage'
            archiveArtifacts(artifacts: 'build/posix_sitl_default/coverage-html/', onlyIfSuccessful: true)
          }
        }
      }
    }
  }
  environment {
    CI = '1'
  }
}