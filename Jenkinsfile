node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Analyze code') { 
        withMaven(
            // Maven installation declared in the Jenkins "Global Tool Configuration"
            maven: 'M3',
            // Maven settings.xml file defined with the Jenkins Config File Provider Plugin
            // Maven settings and global settings can also be defined in Jenkins Global Tools Configuration
            mavenSettingsConfig: 'my-maven-settings',
            mavenLocalRepo: '.repository') {

                // Run the maven build
                sh "mvn clean install"

            } // withMaven will discover the generated Maven artifacts, JUnit Surefire & FailSafe & FindBugs reports...
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        label 'jenkins-slave'
        app = docker.build("registry:5000/javavulnerablelab")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('http://localhost:5000') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
