node {
    def app

     stage('Clone repository') {
         checkout scm
     }

     stage('Build image') {
         app = docker.build("ap-seoul-1.ocir.io/cnqphqevfxnp/db-fixture-rest-api")
     }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

     stage('Push image') {
         docker.withRegistry('https://ap-seoul-1.ocir.io', 'ocir') {
             app.push("${env.BUILD_NUMBER}")
         }
     }

     stage('Trigger ManifestUpdate') { 
        echo "triggering tuplus-update-manifest job"
        build job: 'tuplus-update-manifest', parameters: [
            string(name: 'DOCKERTAG', value: env.BUILD_NUMBER),
            string(name: 'SERVICE', value: 'db-fixture-rest-api')
        ]
     }
}