# Website Uptime Monitor Script

# Configuration
$websiteUrl = "http://yourwebsite.com"
$timeoutSec = 10

# Email configuration for alerts (if website is down)
$smtpServer = "smtp.yourmailserver.com"
$smtpFrom = "your-email@example.com"
$smtpTo = "alert-email@example.com"
$smtpSubject = "Website Down Alert!"
$smtpBodyTemplate = "The website {0} is DOWN as of {1}."

# Function to check website status
function Test-WebsiteStatus {
    param (
        [string]$url
    )

    try {
        $response = Invoke-WebRequest -Uri $url -Method Head -TimeoutSec $timeoutSec -ErrorAction Stop
        return $true
    } catch {
        return $false
    }
}

# Function to send email notification (if website is down)
function Send-AlertEmail {
    param (
        [string]$subject,
        [string]$body
    )

    try {
        $mailmessage = New-Object system.net.mail.mailmessage
        $mailmessage.From = $smtpFrom
        $mailmessage.To.Add($smtpTo)
        $mailmessage.Subject = $subject
        $mailmessage.Body = $body

        $smtp = New-Object system.net.mail.smtpclient($smtpServer)
        # Uncomment and configure if authentication is required
        # $smtp.Credentials = New-Object System.Net.NetworkCredential("username", "password")
        $smtp.Send($mailmessage)
        Write-Output "Alert email sent successfully."
    } catch {
        Write-Output "Failed to send alert email: $_"
    } finally {
        if ($mailmessage) { $mailmessage.Dispose() }
        if ($smtp) { $smtp.Dispose() }
    }
}

# Main logic: check the website status and send an alert if down
if (Test-WebsiteStatus -url $websiteUrl) {
    Write-Output "$websiteUrl is UP!"
} else {
    Write-Output "$websiteUrl is DOWN. Sending alert..."
    $smtpBody = [string]::Format($smtpBodyTemplate, $websiteUrl, (Get-Date))
    Send-AlertEmail -subject $smtpSubject -body $smtpBody
}