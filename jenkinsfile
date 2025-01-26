node {
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
