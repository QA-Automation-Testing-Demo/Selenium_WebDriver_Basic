pipeline {
    agent any

    // Example environment variables (demo only)
    environment {
        CHROME_VERSION       = 'YOUR_CHROME_VERSION_HERE'
        CHROMEDRIVER_VERSION = 'YOUR_CHROMEDRIVER_VERSION_HERE'
        CHROME_INSTALL_PATH  = 'C:\\Program Files\\Google\\Chrome\\Application'
        CHROMEDRIVER_PATH    = 'C:\\Program Files\\Google\\Chrome\\Application\\chromedriver.exe'
    }

    stages {

        stage('Checkout code') {
            steps {
                // TODO: replace ??? with your GitHub account / org
                git branch: 'main',
                    url: 'https://github.com/???/SeleniumBasicExercise.git'
            }
        }

        stage('Set up .NET Core') {
            steps {
                // In workshop: just verify that .NET SDK is installed
                bat '''
echo Checking .NET SDK installation...
dotnet --info
'''
            }
        }

        stage('Uninstall Current Chrome (demo only)') {
            steps {
                bat '''
echo [INFO] Demo step only - NOT uninstalling Chrome on this machine.
'''
            }
        }

        stage('Download and Install ChromeDriver (demo only)') {
            steps {
                bat '''
echo [INFO] Demo step only - NOT downloading ChromeDriver.
echo Would target ChromeDriver version: %CHROMEDRIVER_VERSION%
echo Expected ChromeDriver path: %CHROMEDRIVER_PATH%
'''
            }
        }

        stage('Restore dependencies') {
            steps {
                // Restore dependencies using the solution file
                bat '''
echo Restoring project dependencies...
dotnet restore SeleniumBasicExercise.sln
'''
            }
        }

        stage('Build') {
            steps {
                // Build the project using the solution file
                bat '''
echo Building solution...
dotnet build SeleniumBasicExercise.sln --configuration Release
'''
            }
        }

        stage('Run TestProject1 tests') {
            steps {
                bat '''
echo Running TestProject1 tests...
dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults_TestProject1.trx"
'''
            }
        }

        stage('Run TestProject2 tests') {
            steps {
                bat '''
echo Running TestProject2 tests...
dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults_TestProject2.trx"
'''
            }
        }
    }

    post {
        always {
            // Keep all test result files as build artifacts
            archiveArtifacts artifacts: '**/TestResults/*.*', allowEmptyArchive: true
            // No 'junit' here, because TRX is not JUnit XML.
        }
    }
}
