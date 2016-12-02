node("docker") {
    docker.withRegistry('https://index.docker.io/v1/', 'dvohra-dockerhub') {
        
        sh "pacman -Syv lib32-libstdc++5 lib32-zlib"
        sh "sudo apt-get install git"
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
