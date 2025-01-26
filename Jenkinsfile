node {
    stage('Clone Repository') {
        echo "Cloning the repository"
        git url: 'https://github.com/maruthan64/gcp_secrets_all.git'
    }

    stage('Load Secrets') {
        echo "Loading secrets from secrets.env"
        def secrets = readFile 'gcp_secrets_all/secrets.env'
        def lines = secrets.split('\n')
        lines.each { line ->
            if (line && !line.startsWith('#')) {
                def parts = line.split('=')
                def key = parts[0].trim()
                def value = parts[1].trim()
                env."${key}" = value
            }
        }
    }

    stage('Print Secrets') {
        echo "Printing secrets"
        echo "PROJ_API_KEY: ${env.PROJ_API_KEY}"
        echo "SQL_USERNAME: ${env.SQL_USERNAME}"
        echo "SQL_PASSWORD: ${env.SQL_PASSWORD}"
    }

    stage('Capture Disk Space') {
        echo "Capturing Disk Space Information"
        
        // Capture the output of the df command in a variable
        def diskSpace = sh(script: 'df -h / | tail -n 1', returnStdout: true).trim()
        
        // Parse the output if necessary (e.g., to extract specific values)
        def diskUsed = diskSpace.split()[2] // Gets the used space column
        def diskAvail = diskSpace.split()[3] // Gets the available space column

        // Set the variables as environment variables
        env.DISK_USED = diskUsed
        env.DISK_AVAIL = diskAvail

        // Print the captured variables
        echo "Used Disk Space: ${env.DISK_USED}"
        echo "Available Disk Space: ${env.DISK_AVAIL}"
    }
}