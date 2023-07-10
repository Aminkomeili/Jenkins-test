def services = ['NTP']
VERSION = 1.3

pipeline {
    agent any

    stages {
        stage('Clone ZATS Project') {
            steps {
                sh 'rm -rf ztest zats'
                sh 'git clone git@git.zharfpouyan.net:systemdevnet/ztest.git'
                sh 'git clone git@git.zharfpouyan.net:systemdevnet/zats.git'
            }
        }
        stage('Create ZATS Image') {
            steps {
                dir('zats') {
                    sh "docker image rm -f zats:${VERSION}"
                    sh "docker build -t zats:${VERSION} ."
                }
            }
        }
        stage('Deploy ZATS Latest image') {
            steps {
                echo "docker image tag zats:latest 192.168.70.196:5000/zats:${VERSION}"
                echo "docker image push 192.168.70.196:5000/zats:${VERSION}"
            }
        }
        stage('Service & Routing Tests') {
            steps {
                script {
                    def jobs = [:]
                    for (int i = 0; i < services.size(); i++) {
                        def service = services[i]
                        jobs[service] = {
                            stage("${service}") {
                                dir("ztest/Ztest/Functionality/${service}") {
                                    stage("Build ${service} Service") {
                                        sh "echo 'FROM zats:1.0' >> Dockerfile"
                                        sh "echo 'LABEL \"maintainer\"=\"amin.komeili\"' >> Dockerfile"
                                        sh "echo 'RUN mkdir /${service}/' >> Dockerfile"
                                        sh "echo 'WORKDIR /${service}/' >> Dockerfile"
                                        sh "echo 'COPY . /${service}/' >> Dockerfile"
                                        sh "echo 'CMD python3 \$(find . -name \"*.py\")' >> Dockerfile"
                                        sh "echo 'CMD find . -name \"*.py\"' >> Dockerfile"
                                        sh "docker image rm -f ${service.toLowerCase()}:${VERSION}"
                                        sh "docker build -t ${service.toLowerCase()}:${VERSION} ."
                                        sh "cat Dockerfile"
                                        sleep(1)
                                    }
                                    stage("Deploy ${service} Container") {
                                        echo "docker image tag ${service}:${VERSION} 192.168.70.196:5000/${service}:${VERSION}"
                                        echo "docker image push 192.168.70.196:5000/${service}:${VERSION}"
                                        sleep(1)
                                    }
                                    stage("Test ${service}") {
                                        sh "docker run -itd --name ${service.toLowerCase()} ${service.toLowerCase()}:${VERSION}"
                                        sh "docker stop ${service.toLowerCase()}"
                                        sleep(1)
                                    }
                                    stage("Release ${service} Logs") {
                                        sh "docker logs ${service.toLowerCase()} >> ${service.toLowerCase()}:${VERSION}.log"
                                        sh "docker container rm ${service.toLowerCase()}"
                                        sh "cat ${service.toLowerCase()}:${VERSION}.log"
                                        sleep(1)
                                    }
                                }
                            }
                        }
                    }

                    parallel jobs
                }
            }
        }
        stage('Generating Changelog') {
            steps {
                dir("Automation/upgarde-images") {
                    sh "python3 image.py > ../logs/image.ver"
                    sh "touch ../logs/zats:\$(cat image.ver).log"
                    sh "echo 'changelog will save here' > ../logs/zats:\$(cat image.ver).log "
                }
            }
        }
        stage('Archive Logs') {
            steps {
                dir('ztest') {
                    archiveArtifacts artifacts: '**/*.log', fingerprint: true
                }
                dir('Automation/logs') {
                    archiveArtifacts artifacts: '**/*.log', fingerprint: true
                }
            }
        }
    }
}
