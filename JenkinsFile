node {
	def application = "myproject1image"
	def dockerhubaccountid = "chinmay8292"
	stage('Clone repository') {
		checkout scm
		//checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chinmaypatil8292/walletAPIProject1.git']]])
		sh "mvn -Dmaven.test.failure.ignore=true clean package"
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
	}
	}

	stage('Deploy') {
		sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
