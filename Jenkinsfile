node {

    checkout scm

    // Obtener el id de confirmación que se usará como etiqueta (versión) en la imagen
    sh "git rev-parse --short HEAD > commit-id"
    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    
    // configurar el nombre de la aplicación, la dirección del repositorio y el nombre de la imagen con la versión
    appName = "app"
    registryHost = "127.0.0.1:30400/"
    imageName = "${registryHost}${appName}:${tag}"
    
    // Empezamos..
    
    stage "Build"

        def customImage = docker.build("${imageName}")

    stage "Push"

        customImage.push()
    
    //stage "Unit Test"

        //teste = "fullstack"


    stage "Deploy PROD"

        input "Deploy to PROD?"
        customImage.push('latest')
        sh "kubectl apply -f https://raw.githubusercontent.com/sebateso90/AyiAcademia/master/k8s_app.yaml"
        sh "kubectl set image deployment app app=${imageName} --record"
        sh "kubectl rollout status deployment/app"
}
