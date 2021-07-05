node {

    stage('Startup Clean') {
        sh 'rm -f -r -d -v *'
        sh 'rm -f -r -d -v .[!.]* ..?*'
    }

    stage('cloneRepository') {
        checkout scm
    }

    stage('List nodes') {
        withKubeConfig([credentialsId: 'kubeconfig-cluster2',
                    caCertificate: '',
                    serverUrl: '',
                    contextName: '',
                    clusterName: '',
                    namespace: ''
                    ]) {
            sh 'kubectl get nodes'
        }
    }

    stage('List pods') {
        withKubeConfig([credentialsId: 'kubeconfig-cluster2',
                    caCertificate: '',
                    serverUrl: '',
                    contextName: '',
                    clusterName: '',
                    namespace: ''
                    ]) {
            sh 'kubectl get pods -A'
        }
    }

    stage('Get AporetoC Creds') {
        withCredentials([file(credentialsId: 'aporeto-creds', variable: 'mycreds')]) {
            sh "cp \$mycreds $WORKSPACE/default.creds"
        }       
    }

    stage('Aporeto') {
        sh 'curl -o apoctl https://download.aporeto.com/apoctl/linux/apoctl'
        sh 'chmod +x apoctl'
        withEnv(["APOCTL_CREDS=$WORKSPACE/default.creds"]) {
            sh './apoctl auth verify'  
        }
    }

    stage('End Clean') {
        sh 'rm -f -r -d -v *'
        sh 'rm -f -r -d -v .[!.]* ..?*'
    }
}