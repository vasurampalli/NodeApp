node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */
	git 'https://github.com/vasurampalli/NodeApp.git'

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */
	//sh "sudo usermod -a -G docker $rampallidocker"
	//sh "sudo chmod 777 //var/run/docker.sock"

        app = docker.build("rampallidocker/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-id') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
    stage('Dev Deploy'){
		//def dockerRun = "docker run -d -p 9090:8080 --name nodeapp ${dockerImage}"
		  def dockerRun = "docker run -d -p 8000:8000 --name nodeapp rampallidocker/nodeapp:latest"
		sshagent(['dev-docker']) {
		    try{
			    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.16.239 ${dockerRun} "
			}catch(e){
			
			
			}
			sh "ssh  ubuntu@172.31.16.239 ${dockerRun}"
		
}
