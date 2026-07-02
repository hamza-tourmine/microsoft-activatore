# Optimisation services Windows - mode prudent
# Lance PowerShell en administrateur

$backupPath = "$env:USERPROFILE\Desktop\services_backup.csv"

# Sauvegarde des services actuels
Get-Service | Select-Object Name, DisplayName, Status, StartType |
    Export-Csv -Path $backupPath -NoTypeInformation -Encoding UTF8

Write-Host "Sauvegarde creee ici : $backupPath" -ForegroundColor Green

# Services optionnels courants a mettre en Manuel
$optionalServices = @(
    "AdobeARMservice",
    "AGMService",
    "AGSService",
    "gupdate",
    "gupdatem",
    "MicrosoftEdgeElevationService",
    "edgeupdate",
    "edgeupdatem",
    "OneSyncSvc",
    "XboxGipSvc",
    "XblAuthManager",
    "XblGameSave",
    "XboxNetApiSvc",
    "MapsBroker",
    "Fax",
    "RetailDemo",
    "RemoteRegistry",
    "WerSvc",
    "DiagTrack",
    "dmwappushservice",
    "WMPNetworkSvc",
    "Spooler"
)

foreach ($serviceName in $optionalServices) {
    $service = Get-Service -Name $serviceName -ErrorAction SilentlyContinue

    if ($service) {
        try {
            Stop-Service -Name $serviceName -Force -ErrorAction SilentlyContinue
            Set-Service -Name $serviceName -StartupType Manual
            Write-Host "OK: $serviceName mis en Manuel" -ForegroundColor Yellow
        } catch {
            Write-Host "Impossible de modifier: $serviceName" -ForegroundColor Red
        }
    }
}

Write-Host "Termine. Redemarre ton PC." -ForegroundColor Green
