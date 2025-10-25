# hardware-original-comonent-control-key

# CPU seri numarasını al (Get-CimInstance ile güncellendi)
$cpu = (Get-CimInstance Win32_Processor).ProcessorId

# Anakart (BaseBoard) seri numarasını al (Get-CimInstance ile güncellendi)
$board = (Get-CimInstance Win32_BaseBoard).SerialNumber

# İlk fiziksel diskin seri numarasını al (Get-CimInstance ile güncellendi)
$disk = (Get-CimInstance Win32_PhysicalMedia | Select-Object -First 1).SerialNumber

# BIOS seri numarasını al (Get-CimInstance ile güncellendi)
$bios = (Get-CimInstance Win32_BIOS).SerialNumber

# İlk ekran kartının (GPU) PNP Device ID'sini al (Get-CimInstance ile güncellendi)
$gpu = (Get-CimInstance Win32_VideoController | Select-Object -First 1).PNPDeviceID

# Donanım bilgilerini birleştir
$rawData = "$cpu|$board|$disk|$bios|$gpu"

# SHA-256 hash oluştur
$hashBytes = (New-Object System.Security.Cryptography.SHA256Managed).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($rawData))
$hash = [System.BitConverter]::ToString($hashBytes) -replace "-",""

# Donanım parmak izini yazdır
Write-Host "dev_enes Cihaz Parmak İzi: $hash"
