def services = ['CLI', 'DHCP', 'Interface',  'NTP','PBR', 'VRRP']
def routing = ['ISIS','OSPF','RIP','StaticRoute']

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
        stage('Service Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < services.size(); i++) {
                        def service = services[i]

                        jobs[service] = {
                            stage("${service}") {
                                stage("Build ${service} Service") {
                                    echo "Build ${service}"
                                    sleep(5)
                                }
                                stage("Deploy ${service} Container") {
                                    echo "Deploy ${service}"
                                    sleep(5)
                                }
                                stage("Test ${service}") {
                                    echo "Test ${service}"
                                    sleep(5)
                                }
                                stage("Release ${service} Logs") {
                                    echo "Release ${service}"
                                    sleep(5)
                                }
                            }
                        }
                    }

                    parallel jobs
                }
            }
            
        }
        stage('Routing Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < routing.size(); i++) {
                        def route = routing[i]

                        jobs[service] = {
                            stage("${route}") {
                                stage("Build ${route} Service") {
                                    echo "Build ${service}"
                                    sleep(5)
                                }
                                stage("Deploy ${route} Container") {
                                    echo "Deploy ${route}"
                                    sleep(5)
                                }
                                stage("Test ${route}") {
                                    echo "Test ${route}"
                                    sleep(5)
                                }
                                stage("Release ${route} Logs") {
                                    echo "Release ${route}"
                                    sleep(5)
                                }
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
