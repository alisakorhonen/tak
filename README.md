# tak
Alisa

Työtehtävien automatisointi komentokielellä

Automaattinen työpöydän järjestäjä. 

Tiedostot menevät Downloads kansiosta automaattisesti niiden tyypin mukaan eri kansioihin.

$sourceFolder = "C:\Users\admin\Downloads"
$destinationFolders = @{
    "kuvat" = "C:\kuvat"
    "dokumentit" = "C:\dokumentit"
}
function Move-Files {
    param (
        [string]$Path
    )
    try {
        Write-Output "Siirretään tiedostoa: $Path"
        $file = Get-Item -Path $Path
        switch -Wildcard ($file.Extension) {
            ".png" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
            ".jpg" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
            ".jpeg" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
            ".pdf" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
            ".docx" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
            ".txt" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
            default { Write-Output "Tiedostoa ei siirretty: $($file.Name)" }
        }
    } catch {
        Write-Output "Virhe tiedostoa käsiteltäessä: $Path - $_"
    }
 }
 $watcher = New-Object System.IO.FileSystemWatcher
 $watcher.Path = $sourceFolder
 $watcher.Filter = "*.*"
 $watcher.IncludeSubdirectories = $false
 $watcher.EnableRaisingEvents = $true

 Register-ObjectEvent $watcher "Created" -Action {
     param($source, $e)
     Write-Output "Tiedosto luotu: $($e.FullPath)"
     Move-Files -Path $e.FullPath
 }
 # Pidetään skripti käynnissä
 Write-Output "Skripti käynnissä. Paina Ctrl+C keskeyttääksesi."
 while ($true) { Start-Sleep -Seconds 1 }

{

Salasanan vahvuustarkistus

function Test-PasswordStrength {
    param (
        [string]$Password
    )
 
    $lengthCriteria = $Password.Length -ge 8
    $digitCriteria = $Password -match '\d'
    $specialCharCriteria = $Password -match '[^A-Za-z0-9]'
 
    $strengthScore = ($lengthCriteria + $digitCriteria + $specialCharCriteria)
 
    switch ($strengthScore) {
        3 { "Vahva" }
        2 { "Keskivahva" }
        default { "Heikko" }
    }
}
 
function Improve-Password {
    param (
        [string]$Password
    )
 
    $suggestions = @()
 
    if ($Password.Length -lt 8) {
        $suggestions += "Tee salasanasta vähintään 8 merkkiä pitkä."
    }
    if ($Password -notmatch '\d') {
        $suggestions += "Lisää ainakin yksi numero."
    }
    if ($Password -notmatch '[^A-Za-z0-9]') {
        $suggestions += "Lisää ainakin yksi erikoismerkki, kuten !, @, #, $."
    }
 
    if ($suggestions.Count -eq 0) {
        $suggestions += "Salasana on vahva."
    }
 
    return $suggestions
}
 
$Password = Read-Host -Prompt "Anna salasana"
$strength = Test-PasswordStrength -Password $Password
Write-Output "Salasanan vahvuus: $strength"
 
$suggestions = Improve-Password -Password $Password
Write-Output "Vinkkejä salasanan parantamiseen:"
$suggestions | ForEach-Object { Write-Output "- $_" }
 
 }
