node("docker") {
    docker.withRegistry('https://index.docker.io/v1/', 'dvohra-dockerhub') {
        deleteDir()

        sh "wget https://github.com/git/git/archive/v2.11.0.tar.gz"
        sh  "tar -zxf git-2.11.0.tar.gz"
        sh   "cd git-2.11.0"
        sh   "make configure"
        sh  "./configure --prefix=/usr"
        sh   "make all doc info"
        sh  "sudo make install install-doc install-html install-info"
        sh "git --version"

      
        git url: "https://github.com/dvohra/mysql.git", credentialsId: 'dvohra-github'
    
        sh "git rev-parse HEAD > .git/commit-id"
        def commit_id = readFile('.git/commit-id').trim()
        println commit_id
    
        stage "build"
        def app = docker.build "dvohra/mysql"
    
        stage "publish"
        app.push 'master'
        app.push "${commit_id}"
    }
}
