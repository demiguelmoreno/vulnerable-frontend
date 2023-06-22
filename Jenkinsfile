node {

environment {
  PRISMA_API_URL='https://api.eu.prismacloud.io'
}

stage('Clone repository') {
  checkout scm       
}

stage('Scan IaC Terraform template files') {
  script { 
    docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''")  {     
      try {
        sh 'checkov -d ./terraform --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --prisma-api-url https://api.eu.prismacloud.io --bc-api-key ${ACCESS_KEY}::${SECRET_KEY} --repo-id demiguelmoreno/vulnerable-frontend --branch main'
          junit skipPublishingChecks: true, testResults: 'results.xml'
      } catch (err) {
        junit skipPublishingChecks: true, testResults: 'results.xml'
        throw err
      }
    }
  }  
}


stage('Scan SCA from pom.xml file') {
    script { 
        docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''")  {     
            try {
                sh 'checkov -f pom.xml --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --prisma-api-url https://api.eu.prismacloud.io  --bc-api-key ${ACCESS_KEY}::${SECRET_KEY} --repo-id demiguelmoreno/evil.petclinic.sca --branch main'
                junit skipPublishingChecks: true, testResults: 'results.xml'
            } catch (err) {
                junit skipPublishingChecks: true, testResults: 'results.xml'
                throw err
            }
        }  
    }  
}

stage('Build image') {
  /// This step simulates the image build where the source code plus open source packages are used with Dockerfile to create the image
  echo 'Building image...' 
}

stage('Scan image') {
  /// This step simulates the image scan
  echo 'Scanning image...'
}

stage('Sandbox image') {
  /// This step simulates the image sandbox
  echo 'Sandboxing image...'
}


//stage('Scan image before Pushing to Registry') {
//    try {
//        sh 'docker pull demiguelmoreno/evilpetclinic:latest'  
//        withCredentials([usernamePassword(credentialsId: 'twistlock_creds', passwordVariable: 'TL_PASS', usernameVariable: 'TL_USER')]) {
//            prismaCloudScanImage ca: '', cert: '', dockerAddress: 'unix:///var/run/docker.sock', ignoreImageBuildTime: true, image: 'demiguelmoreno/evilpetclinic:latest', key: '', logLevel: 'debug', podmanPath: '', project: '', resultsFile: 'prisma-cloud-scan-results.json'
//        }
//    } finally {
//        prismaCloudPublish resultsFilePattern: 'prisma-cloud-scan-results.json'
//    }
//}

stage('Push image to the registry') {
    /// This step simulates the image Push to the registry 
    echo 'Pushing image to registry...'
}

/// Optional stage to check how Dockerfile scanning works
 
/// stage('Scan Dockerfile') {
/// script { 
///       docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''")  {     
///            try {
///               sh 'checkov -f Dockerfile --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --prisma-api-url https://api.eu.prismacloud.io  --bc-api-key ${ACCESS_KEY}::${SECRET_KEY} --repo-id demiguelmoreno/evil.Dockerfile --branch main'
///               junit skipPublishingChecks: true, testResults: 'results.xml'
///              } catch (err) {
///                  junit skipPublishingChecks: true, testResults: 'results.xml'
///                  throw err
///              }
///   }  
/// }  
///   }
 
stage('Scan k8s manifest') {
    script { 
        docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''")  {     
            try {
                sh 'checkov -f deploy.yaml --use-enforcement-rules -o cli -o junitxml --output-file-path console,results.xml --prisma-api-url https://api.eu.prismacloud.io  --bc-api-key ${ACCESS_KEY}::${SECRET_KEY} --repo-id demiguelmoreno/evil.yaml --branch main'
                junit skipPublishingChecks: true, testResults: 'results.xml'
            } catch (err) {
                junit skipPublishingChecks: true, testResults: 'results.xml'
                throw err
            }
        }
    }  
}

/// END NODE
}
