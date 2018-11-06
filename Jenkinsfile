podTemplate(label: 'appsec', name: 'appsec', namespace: 'jenkins-ns', containers: [
        containerTemplate(alwaysPullImage: true, envVars: [secretEnvVar(key: 'DOJO_API_KEY', secretKey: 'dojo-api-key', secretName: 'dependency-check-secrets'), envVar(key: 'DOJO_URL', value: 'https://defect-dojo.azurewebsites.net'), envVar(key: 'DOJO_ENGAGEMENT_ID', value: '1'), envVar(key: 'SOURCE_REPO', value: 'https://github.com/RiieCco/assessment.git')], image: 'rtencatexebia/dependency-check', name: 'dependency-check', privileged: false, ttyEnabled: false,)
    ]) {
    node('appsec') {
        stage('Check dependencies') {
            container('dependency-check') {
            }
        }
    }
}
