// pipeline {
//     agent none
    
//     stages {
//         stage('Build') {
//             agent {
//                 docker {
//                     image 'python:2-alpine'
//                 }
//             }
//             steps {
//                 sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//             }
//         }
        
//         stage('Test') {
//             agent {
//                 docker {
//                     image 'qnib/pytest'
//                 }
//             }
//             steps {
//                 sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
//             }
//             post {
//                 always {
//                     junit 'test-reports/results.xml'
//                 }
//             }
//         }
        
//         stage('Deploy') {
//             agent {
//                 docker {
//                     image 'python:3.9'
//                     args '-u root'
//                 }
//             }
//             steps {
//                 sh 'pip install pyinstaller'
//                 sh 'pyinstaller --onefile sources/add2vals.py'
//                 sleep time: 1, unit: 'MINUTES'
//                 echo 'Pipeline has finished successfully.'
//             }
//             post {
//                 success {
//                     archiveArtifacts 'dist/add2vals'
//                 }
//             }
//         }
//     }
// }


node {
    def dockerImage = 'python:3.9'
    def dockerArgs = '-u root'
    
    docker.image(dockerImage).inside(dockerArgs) {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
        
        stage('Test') {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
        
        stage('Deploy') {
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            sleep 60
            echo 'Pipeline has finished successfully.'
            archiveArtifacts artifacts: 'dist/add2vals', fingerprint: true
        }
    }
}
