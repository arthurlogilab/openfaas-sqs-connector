def label = "docker-${UUID.randomUUID().toString()}"

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) {

  def image = "jenkins/jnlp-slave"
  node(label) {
    stage('Build Docker and Push image') {
      container('docker') {
        checkout scm
        sh "docker build -t affixxx/openfaas-sqs-connector:latest ."
        if(env.BRANCH_NAME == "master") {
          withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
            sh "docker push affixxx/openfaas-sqs-connector:latest"
          }
        }
      }
    }
  }
}
