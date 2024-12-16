# tak
Alisa

Työtehtävien automatisointi komentokielellä

Automaattinen työpöydän järjestäjä. Tiedostot menevät niiden tyypin mukaan eri kansioihin esimerkiksi kuvat menevät kuvat kansioon ja dokumentit menevät toiseen kansioon.

# Määritellään lähdekansio ja kohdekansiot
$sourceFolder = "C:\Users\admin\Desktop"  # Korvaa tämä oikealla polulla
$destinationFolders = @{
    "kuvat" = "C:\kuvat"
    "dokumentit" = "C:\dokumentit"
}
 
# Käydään läpi kaikki tiedostot lähdekansiossa
Get-ChildItem -Path $sourceFolder | ForEach-Object {
    $file = $_
    switch -Wildcard ($file.Extension) {
        ".png" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
        ".jpg" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
        ".jpeg" { Move-Item -Path $file.FullName -Destination $destinationFolders["kuvat"] }
        ".pdf" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
        ".docx" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
        ".txt" { Move-Item -Path $file.FullName -Destination $destinationFolders["dokumentit"] }
        Default { Write-Output "Tiedostoa ei siirretty: $($file.Name)" }
    }
}
 
Write-Output "Tiedostot siirretty onnistuneesti!"


Automaattinen taustakuva vaihtaja. Koneen taustakuva vaihtuu toiseen taustakuvaan tietyn aikavälin ajoin.
