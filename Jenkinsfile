pipeline {
    agent any
    environment {
        REPORTS_DIR = 'D:\\Jenkins-pipeline\\Thread Group-simple\\reports'
        JMX_FILE = 'D:\\Jenkins-pipeline\\Thread Group-simple.jmx'
    }
    triggers{
        cron('H 5 * * 1-5')
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/aparnakurupr/Jenkins-pipeline.git'
            }
        }
        stage('Run JMeter Test') {
            steps {
                script {
                    bat "mkdir ${REPORTS_DIR}"
                    bat "jmeter -j jmeter.save.saveservice.output_format=xml -n -t ${JMX_FILE} -l ${REPORTS_DIR}/results.jtl -e -o ${REPORTS_DIR}/html-report"
                }
            }
        }
        stage('Publish Performance Report') {
            steps {
                script {
                    if (fileExists("${REPORTS_DIR}/results.jtl")) {
                        performanceReport parsers: [[$class: 'JMeterCsvParser', glob: "${REPORTS_DIR}/results.jtl"]]
                    } else {
                        echo "JMeter results file not found: ${REPORTS_DIR}/results.jtl"
                    }
                }
            }
        }
        // stage('Archive Artifacts') {
        //     steps {
        //         // Archive the generated .jtl and HTML reports
        //         archiveArtifacts artifacts: "${REPORTS_DIR}/**", allowEmptyArchive: true
        //     }
        // }
    }
}













