def routing = ['ISIS', 'OSPF', 'RIP', 'StaticRoute']
def services = ['CLI', 'DHCP', 'Interface', 'NTP', 'PBR', 'VRRP']

pipeline {
    agent any

    stages {
        stage('Create virtual environment') {
            steps {
                echo "virtual evn created"
            }
        }
        stage('Activate virtual environment') {
            steps {
                echo "Virtual env activated"
            }
        }
        stage('Install requirements') {
            steps {
                echo "requirements installed"
            }
        }
        stage('Routing Protocol Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < routing.size(); i++) {
                        def route = routing[i]

                        jobs[route] = {
                            stage("Build ${route}") {
                                echo "Build ${route}"
                                sleep(1)
                            }
                            stage("Deploy ${route}") {
                                echo "Deploy ${route}"
                                sleep(1)
                            }
                            stage("Run ${route}") {
                                echo "Run ${route}"
                                sleep(1)
                            }
                            stage("Logging ${route}") {
                                echo "Logging ${route}"
                                sleep(1)
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
                            stage("Build ${service}") {
                                echo "Build ${service}"
                                sleep(1)
                            }
                            stage("Deploy ${service}") {
                                echo "Deploy ${service}"
                                sleep(1)
                            }
                            stage("Run ${service}") {
                                echo "Run ${service}"
                                sleep(1)
                            }
                            stage("Logging ${service}") {
                                echo "Logging ${service}"
                                sleep(1)
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
