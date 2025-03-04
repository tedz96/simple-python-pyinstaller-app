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
            sh 'apt update && apt install openssh-client -y'
            withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'SSH_KEY')]) {
                sh 'scp -o StrictHostKeyChecking=no -i $SSH_KEY dist/add2vals ubuntu@18.139.208.22:/home/ubuntu'
            }
            echo 'Menunggu selama 1 menit agar aplikasi bisa digunakan...'
            sleep time: 60, unit: 'SECONDS' 
        }
    }
}
