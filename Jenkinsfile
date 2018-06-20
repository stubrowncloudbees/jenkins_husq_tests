podTemplate(label: "kubernetes", yaml: """
apiVersion: v1
kind: Pod
metadata:
  name: docker
spec:
  containers:
  - name: docker
    image: docker:18.03-git
    command: ["sleep"]
    args: ["100000"]
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-socket
  volumes:
  - name: docker-socket
    hostPath:
      path: /var/run/docker.sock
      type: Socket
"""
) {
    node("kubernetes") {
        container("docker") {
            checkout scm
            stage("something") {
               withCredentials([usernamePassword(credentialsId: 'dockerhubstu', passwordVariable: 'PASSWORD', usernameVariable: 'USER')]) {
   
                sh 'docker login -p ${PASSWORD} -u ${USER}' 
                sh "docker version"
                sh "docker build ."
               }
            }
        }
    }
}
