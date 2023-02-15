library identifier: "pipeline-library@v1.5",
retriever: modernSCM(
  [
    $class: "GitSCMSource",
    remote: "https://github.com/redhat-cop/pipeline-library.git" 
  ]
)

appName = "node-app-backend-t"

pipeline {
    agent any
  environment{
    COMMIT_ID = sh (script: 'git rev-parse HEAD', returnStdout: true).substring(0,5)
  }
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        stage("Docker Build") {
            steps {
                binaryBuild(buildConfigName: appName, buildFromPath: ".")
            }
        }
      stage("Tag image") {
       steps{
    tagImage([
            sourceImagePath: "dhanya-jenkins",
            sourceImageName: "docker.io/dhanyashree/node-server",
            sourceImageTag : "latest",
            toImagePath: "dhanya-jenkins",
            toImageName    : "docker.io/dhanyashree/node-server",
            toImageTag     : "${COMMIT_ID}"
      
    ])
       }
}
//       stage('deploy') {
//         steps {
//             script {
//                 openshift.withCluster() {
//                     openshift.withProject("$PROJECT_NAME") {
//                         echo "Using project: ${openshift.project()}"
//                          sh 'oc project "$PROJECT_NAME" '
//                          sh 'oc apply -f pod.yaml'
//                     }
//                 }
//             }
//     }
//  }
   }
}
