def routing = ['ISIS', 'OSPF', 'RIP', 'StaticRoute']
def services = ['CLI', 'DHCP', 'Interface', 'NTP','PBR', 'VRRP']

pipeline {
    agent any

    stages {
        stage('Create virtual environment') {
            steps {
                echo 'python3 -m venv myenv'
            }
        }
        stage('Activate virtual environment') {
            steps {
                echo '. myenv/bin/activate'
            }
        }
        stage('Install requirements') {
            steps {
                echo 'pip install -r requirements.txt'
            }
        }
        stage('Routing Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < routing.size(); i++) {
                        def route = routing[i]

                        jobs[route] = {
                            stage("Build ${route}") {
                                echo "Build ${route}"
                                sleep(5)
                                echo "Deploy ${route}"
                                sleep(5)
                                echo "Test ${route}"
                                sleep(5)
                                echo "Release ${route}"
                                sleep(5)
                            }
                            stage("Deploy ${route}") {
                                echo "Build ${route}"
                                sleep(5)
                                echo "Deploy ${route}"
                                sleep(5)
                                echo "Test ${route}"
                                sleep(5)
                                echo "Release ${route}"
                                sleep(5)
                            }
                        }
                    }

                    parallel jobs
                }
            }
        }
        stage('Service Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < services.size(); i++) {
                        def service = services[i]

                        jobs[service] = {
                            stage("${service}") {
                                echo "Build ${service}"
                                sleep(5)
                                echo "Deploy ${service}"
                                sleep(5)
                                echo "Test ${service}"
                                sleep(5)
                                echo "Release ${service}"
                                sleep(5)
                            }
                        }
                    }

                    parallel jobs
                }
            }
        }
        stage('Tagging') {
            steps {
                echo "All tests have been built"
                sleep(5)
            }
        }
        stage('Generating Changelog') {
            steps {
                echo "All tests have been completed"
                sleep(5)
            }
        }
        stage('Create Gitlab Report') {
            steps {
                echo "All tests have been completed"
            }
        }
    }
}
