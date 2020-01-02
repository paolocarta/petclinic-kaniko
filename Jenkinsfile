
pipeline {
  agent {
    kubernetes {
        label 'kaniko-build-pod'
        yamlFile 'podTemplate.yaml'
    }
  }
  stages {
    stage('Maven Install') {
      steps {
        container('maven') {
          //error 'Another fake error'
          sh 'mvn clean install'
          //writeFile file: "application.sh", text: "echo my Built ${BUILD_ID} of ${JOB_NAME}"
          //archiveArtifacts 'application.sh'
          //gateProducesArtifact file: 'application.sh', label: 'Dummy artifact to be consumed by Deploy (master branch) gate'
          //gateProducesArtifact id: 'kaniko', type: 'docker'
        }
      }
    }
    stage('Docker Build and Push') {
      steps {
        container(name: 'kaniko', shell: '/busybox/sh') {
          //error 'Fake error to force failure in Build'
          echo "building artifact"
          sh """#!/busybox/sh
                /kaniko/executor --context `pwd` --destination eu.gcr.io/ci-cd-playground/petclinic-kaniko:latest --cache=true
          """
          publishEvent simpleEvent('dcanadillas/kaniko-petclinic:latest')
        }
      }
    }
  }
}

