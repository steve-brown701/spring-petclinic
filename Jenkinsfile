node ('ubuntu_node1') {
  stage ('Checkout') {
    echo "Cloning git repo."
    git 'https://github.com/steve-brown701/spring-petclinic.git'
  }
  stage ('Build') {
    sh 'mvn clean compile'
  }
  stage ('Test') {
    sh 'mvn test'
    junit '**/target/surefire-reports/TEST-*.xml'
  }
  stage ('Package') {
    sh 'mvn package'
  }
}
