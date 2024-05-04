def pipelineContext = [:]
node {
   def registryProjet='registry.gitlab.com/MohamedAlibousleh/jenkinsdocker/container_registry'
   def IMAGE="${registryProjet}:version-${env.BUILD_ID}"
    stage('Clone') {
          git 'https://github.com/MohamedAli-Bousleh/WebApp-BuildDocker.git'
    }
    stage('Maven package'){
            sh 'mvn package'
    }
    def img = stage('Build') {
          docker.build("$IMAGE",  '.')
    }
    stage('Run') {
            img.withRun("--name run-$BUILD_ID -p 8082:8080") { c ->
            sh 'docker ps'
            sh 'netstat -ntaup'
            sh 'sleep 30s'
            sh 'curl 192.168.10.10:8082'
            sh 'docker ps'
          }
    }
    stage('Push') {
          docker.withRegistry('https://registry.gitlab.com', 'reg1') {
              img.push 'latest'
              img.push()
          }
    }
}
