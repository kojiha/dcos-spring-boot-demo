def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}

node {
    // Checkout source code from Git
    stage 'Checkout'
    checkout scm

    // Build the Java project
    stage 'Build: Code'
    sh '/bin/sh ./mvnw package'

    // Build Docker image
    stage 'Build: Docker Image'
    sh "docker build -t gitlab.app.dcos.local:50000/dcos/spring-boot-demo:${gitCommit()} -t gitlab.app.dcos.local:50000/dcos/spring-boot-demo:latest ."

    // Log in and push image to GitLab
    stage 'Publish'
    withCredentials(
        [[
            $class: 'UsernamePasswordMultiBinding',
            credentialsId: 'gitlab',
            passwordVariable: 'GITLAB_PASSWORD',
            usernameVariable: 'GITLAB_USERNAME'
        ]]
    ) {
        sh "docker login -u ${env.GITLAB_USERNAME} -p ${env.GITLAB_PASSWORD} gitlab.app.dcos.local:50000"
        sh "docker push gitlab.app.dcos.local:50000/dcos/spring-boot-demo:${gitCommit()}"
        sh "docker push gitlab.app.dcos.local:50000/dcos/spring-boot-demo:latest"
    }

    // Deploy
    stage 'Deploy'
    marathon(
        url: 'http://marathon.mesos:8080',
        forceUpdate: true,
        filename: 'marathon.json',
        id: 'test/spring-boot-demo',
        docker: "gitlab.app.dcos.local:50000/dcos/spring-boot-demo:${gitCommit()}".toString()
    )
}