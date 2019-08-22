node {
    // checkout code from scm
    checkout scm
    
    // parse info from values.yaml of helm
    def chart = readYaml file: './guestbook/Chart.yaml'
    def values = readYaml file: './guestbook/values.yaml'

    appName = chart.name
    version = chart.version
    imageTagPrefix = values.imageTagPrefix

    stage "Build"
        sh "docker build -t ${imageTagPrefix}/${appName}_frontend:${version} -f src/php-redis/Dockerfile src/php-redis"
        sh "docker build -t ${imageTagPrefix}/${appName}_redisslave:${version} -f src/redis-slave/Dockerfile src/redis-slave"

    stage "Push"
        sh "docker push ${imageTagPrefix}/${appName}_frontend:${version}"
        sh "docker push ${imageTagPrefix}/${appName}_redisslave:${version}"

    //stage "Deploy"

        // kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'
        // sh "kubectl apply -f guestbook-all-in-one.yaml"

}

