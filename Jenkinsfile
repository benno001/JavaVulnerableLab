podTemplate(label: 'appsec', 
            name: 'appsec', 
            namespace: 'jenkins-ns', 
            podRetention: always(),
            containers: [
        // Dependency check
        containerTemplate(name: 'dependency-check', 
                          image: 'rtencatexebia/dependency-check', 
                          alwaysPullImage: true, 
                          envVars: [
                                  secretEnvVar(key: 'DOJO_API_KEY', secretKey: 'dojo-api-key', secretName: 'defect-dojo-secrets'),
                                  envVar(key: 'DOJO_URL', value: 'https://defect-dojo.azurewebsites.net'),
                                  envVar(key: 'DOJO_ENGAGEMENT_ID', value: '1'),
                                  envVar(key: 'SOURCE_REPO', value: 'https://github.com/RiieCco/assessment.git')
                          ],
                          privileged: false, 
                          ttyEnabled: false,),
        
        // OWASP ZAP (DAST)
        containerTemplate(name: 'zap',
                          image: 'rtencatexebia/owasp-zap', 
                          alwaysPullImage: true, 
                          command: 'zap-baseline.py',
                          ports: [portMapping(name: 'proxy', containerPort: 8080, hostPort: 8080)],
                          args: '-t $(target) -U $(dojo-url) -A $(dojo-api-key) -I $(dojo-engagement-id) -z -addoninstallall',
                          envVars: [
                                  secretEnvVar(key: 'dojo-api-key', secretKey: 'dojo-api-key', secretName: 'defect-dojo-secrets'),
                                  envVar(key: 'dojo-url', value: 'https://defect-dojo.azurewebsites.net'),
                                  envVar(key: 'dojo-engagement-id', value: '1'),
                                  envVar(key: 'target', value: 'https://securityknowledgeframework.org')
                          ], 
                          privileged: false, 
                          ttyEnabled: false,)
        
        // Bandit
        //containerTemplate(alwaysPullImage: true, envVars: [secretEnvVar(key: 'DOJO_API_KEY', secretKey: 'dojo-api-key', secretName: 'defect-dojo-secrets'), envVar(key: 'DOJO_URL', value: 'https://defect-dojo.azurewebsites.net'), envVar(key: 'DOJO_ENGAGEMENT_ID', value: '1'), envVar(key: 'SOURCE_REPO', value: 'https://github.com/RiieCco/assessment.git')], image: 'xebiasecurity/bandit-defectdojo', name: 'bandit', privileged: false, ttyEnabled: false,)
    ]) {
    node('appsec') {
        stage('Check dependencies') {
            container('dependency-check') {
            }
        }
        stage('SAST') {
            container('zap') {
            }
        }
    }
}
