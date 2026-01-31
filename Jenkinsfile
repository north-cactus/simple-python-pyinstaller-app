pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py' 
                stash(name: 'compiled-results', includes: 'sources/*.py*') 
            }
        }
        stage('Test') {
            steps {
                sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh "pyinstaller --onedir --noconfirm sources/add2vals.py"
		sh "tar cvfz dist/add2vals.tar.gz dist/add2vals"
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals.tar.gz'
                }
            }
        }
    }
}
