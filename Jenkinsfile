node {
    def server = Artifactory.server('systtekcloud.jfrog.io')
    def rtGradle = Artifactory.newGradleBuild()
    def buildInfo = Artifactory.newBuildInfo()
    
    stage 'Complicacion/Build'
        git branch: 'main', url: 'https://github.com/systtekcloud/jfrog-artifactory.git'

    stage 'Configuracion Artifactory'
        rtGradle.tool = 'gradle' // Como le asignamos al nombre de la herramienta en Jenkins en configuración
        rtGradle.deployer repo:'project-gradle-dev-local',  server: server
        rtGradle.resolver repo:'project-gradle-dev', server: server

        stage('Configuracion buildInfo') {
            buildInfo.env.capture = true
            buildInfo.env.filter.addInclude("*")
        }

        stage('Configuraciones extra de gradle') {
            rtGradle.usesPlugin = true // El plugin ya está definifo en el build script
        }
        stage('Ejecutar Gradle') {
            rtGradle.run rootDir: "artifactory/", tasks: 'clean artifactoryPublish', buildInfo: buildInfo
        }
        stage('Publicar buildInfo') {
            server.publishBuildInfo buildInfo
        }
}