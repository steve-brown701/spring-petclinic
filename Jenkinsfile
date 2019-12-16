node ('master') {
  def MVN = tool name: 'maven-3.6.3', type: 'maven'
  stage ('Checkout') {
    echo "Cloning git repo."
    git 'https://github.com/steve-brown701/spring-petclinic.git'
  }
  stage ('Build') {
    echo "Building project."
    sh "${MVN}/mvn clean compile"
  }
  stage ('Test') {
    echo "Testing project"
    sh "${MVN}/mvn test"
    junit '**/target/surefire-reports/TEST-*.xml'
  }
  stage ('Package') {
    echo "Packaging project"
    sh "${MVN}/mvn package"
  }
}
