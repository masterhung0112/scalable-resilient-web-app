pipeline {
  agent none
  stages {
    stage('Build Images') {
      parallel {
        stage('Build GCE Image with Packer') {
			    agent {
            docker { image 'hashicorp/packer:full' }
          }
          steps {
              sh """
                PROJECT_ID=`wget -O- 'http://metadata/computeMetadata/v1/project/project-id' --header 'Metadata-Flavor: Google'`
                BRANCH_NAME=${GIT_BRANCH}
                packer build \
                -var "project_id=\${PROJECT_ID}" \
                -var "git_commit=${GIT_COMMIT}" \
                -var "git_branch=\${BRANCH_NAME#*/}" \
                packer.json
              """
            }
          
        }
        // stage('Build and push image with Cloud Build') {
		    //   agent {
        //         docker { image 'gcr.io/cloud-builders/gcloud' }
        //     }
        //   steps {
        //       sh """
        //       PROJECT_ID=`curl -s 'http://metadata/computeMetadata/v1/project/project-id' -H 'Metadata-Flavor: Google'`
        //       BRANCH_NAME=${GIT_BRANCH}
        //       IMAGE_TAG=gcr.io/\${PROJECT_ID}/redmine:\${BRANCH_NAME#*/}-${GIT_COMMIT}
        //       PYTHONUNBUFFERED=1 gcloud builds submit -t \${IMAGE_TAG} .
        //       """
            
        //   }
        // }
      }
    }
  }
}
