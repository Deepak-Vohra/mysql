node("docker") {
    docker.withRegistry('https://index.docker.io/v1/', 'dvohra-dockerhub') {
        deleteDir()

        sh "wget https://github.com/git/git/archive/v2.11.0.tar.gz
            tar -zxf git-2.11.0.tar.gz
            cd git-2.11.0
            make configure
            ./configure --prefix=/usr
            make all doc info
            sudo make install install-doc install-html install-info"
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
