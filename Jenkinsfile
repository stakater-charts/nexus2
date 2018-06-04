#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String nexusPackageName = ""
String nexusStoragePackageName = ""
String nexusChartName = "nexus2"
String nexusStorageChartName = "nexus2-storage"

toolsNode(toolsImage: 'stakater/pipeline-tools:dev') {
    container(name: 'tools') {
        def helm = new io.stakater.charts.Helm()
        def common = new io.stakater.Common()
        def chartManager = new io.stakater.charts.ChartManager()
        stage('Checkout') {
            checkout scm
        }

        stage('Init Helm') {
            helm.init(true)
        }

        stage('Prepare Chart') {
            helm.lint(WORKSPACE, nexusChartName)
            nexusPackageName = helm.package(WORKSPACE, nexusChartName)

            helm.lint(WORKSPACE, nexusStorageChartName)
            nexusStoragePackageName = helm.package(WORKSPACE, nexusStorageChartName)
        }

        stage('Upload Chart') {
            String cmUsername = common.getEnvValue('CHARTMUSEUM_USERNAME')
            String cmPassword = common.getEnvValue('CHARTMUSEUM_PASSWORD')
            chartManager.uploadToChartMuseum(WORKSPACE, nexusChartName, nexusPackageName, cmUsername, cmPassword)
            chartManager.uploadToChartMuseum(WORKSPACE, nexusStorageChartName, nexusStoragePackageName, cmUsername, cmPassword)
        }
    }
}
