// Jenkinsfile to use for ARCH workflow
node {
    def targetRegistry = env.DOCKER_STAGING_REGISTRY ?: 'docker-staging-local.chip-repo.childrens.harvard.edu'
    def imageName = env.JDK_IMAGE ?: 'ihlchip/zulu-openjdk-s6'
    def imageMajorVersion = env.JDK_MAJOR_VERSION ?: '8-latest'
    def imageMinorVersion = env.JDK_MINOR_VERSION ?: '8.152'
    def baseImageName = env.ALPINE_GLIBC_IMAGE_NAME ?: 'ihlchip/alpine37-glibc'
    def image
    stage('Clone repository') {
        checkout scm
    }
    stage('Build and tag image') {
        // Use the provided build script
        sh "docker build -t ${imageName} --build-arg BASE_IMAGE=${baseImageName} ./alpine/8-latest/"
        image = docker.image(imageName);
    }
    stage('Push image') {
        docker.withRegistry("https://${targetRegistry}", 'jenkins-chip-repo') {
            image.push()
            image.push("${imageMajorVersion}")
            image.push("${imageMinorVersion}")
        }
    }
}