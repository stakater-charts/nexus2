#!/usr/bin/groovy
@Library('github.com/stakater/fabric8-pipeline-library@master')

String nexusPackageName = ""
String nexusStoragePackageName = ""
String nexusChartName = "nexus"
String nexusStorageChartName = "nexus-storage"

clientsNode(clientsImage: 'stakater/kops-ansible:helm-bundle') {
    container(name: 'clients') {
        def helm = new io.stakater.charts.Helm()
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
            chartManager.uploadToChartMuseum(WORKSPACE, nexusChartName, nexusPackageName)
            chartManager.uploadToChartMuseum(WORKSPACE, nexusStorageChartName, nexusStoragePackageName)
        }
    }
}
