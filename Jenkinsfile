def REGISTRY_URL='    default-route-openshift-image-registry.apps.tz-205307.cecc.ihost.com'

podTemplate([
    label: 'non-root-jenkins-agent-base',
    cloud: 'openshift',
    serviceAccount: 'jenkins',
    podRetention: onFailure(),
    idleMinutes: '30',
    containers: [
        containerTemplate(
            name: 'jnlp',
            image: "${REGISTRY_URL}/non-root-image-builds/non-root-jenkins-agent-base:latest",
            alwaysPullImage: true,
            args: '${computer.jnlpmac} ${computer.name}'
        )
    ]
]) {
    node('non-root-jenkins-agent-base') {
        git url: 'https://github.com/elcicd/Test-CICD1.git',
            branch: 'development'

        echo ''
        echo '========================='
        echo ''
        echo 'Compiling, testing, and scanning of source would run here'
        echo ''
        echo '========================='
        echo ''

        echo '========== START podman login and build ==============='
        sh """
            podman login --tls-verify=false -u jenkins -p \$(oc whoami -t) ${REGISTRY_URL}

            podman build --tls-verify=false --squash -t ${REGISTRY_URL}/non-root-image-builds/test-cicd1 -f ./Dockerfile
        """
        echo '========== END podman login and build ==============='

        echo ''
        echo '========================='
        echo ''
        echo 'Image scan would run here'
        echo ''
        echo '========================='
        echo ''

        echo '========== START podman push and deploy image from registry; verify USER id is non-root 1001 ==============='
        sh """
            podman push --tls-verify=false ${REGISTRY_URL}/non-root-image-builds/test-cicd1:latest

            oc new-app --image='${REGISTRY_URL}/non-root-image-builds/test-cicd1'

            id
        """
        echo '========== END podman push and deploy image from registry; verify USER id is non-root 1001 ==============='
    }
}