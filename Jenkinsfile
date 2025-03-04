node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    
    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml' 
        }
    }
    stage('Manual Approval'){
        input message: 'Lanjutkan ke tahap Deploy?'
    }
    stage ('Deploy'){
        docker.image('python:3.9').inside('-u root') {
           sh 'pip install pyinstaller'
           sh 'pyinstaller --onefile sources/add2vals.py'
        }
    }
}
