# tak
Emilia

Työtehtävien automatisointi komentokielellä

Salasanan vahvuustarkistus. Tarkoitus on tehdä työkalu, joka tarkistaa kuinka vahva salasana on. Se voi tarkastella salasanan pituuden, erikoismerkit ja numerot. Se antaa vinkkejä miten tehdä heikoista salasanoista vahvempia.


function Test-PasswordStrength {
     param (
         [string]$password
     )
     $length = $password.Length
     $hasUpperCase = $password -cmatch '[A-Z]'
     $hasLowerCase = $password -cmatch '[a-z]'
     $hasDigit = $password -cmatch '\d'
     $hasSpecialChar = $password -cmatch '[^a-zA-Z0-9]'

     $strength = 0
     $strengthTips = @()

     if ($length -ge 8) {
         $strength++
     } else {
         $strengthTips += "Lisää pituutta (vähintään 8 merkkiä)."
     }
     if ($hasUpperCase) {
         $strength++
     } else {
         $strengthTips += "Lisää isoja kirjaimia."
     }
     if ($hasLowerCase) {
         $strength++
     } else {
         $strengthTips += "Lisää pieniä kirjaimia."
     }
     if ($hasDigit) {
         $strength++
     } else {
         $strengthTips += "Lisää numeroita."
     }
     if ($hasSpecialChar) {
         $strength++
     } else {
         $strengthTips += "Lisää erikoismerkkejä (!, @, #, $)."
     }

     switch ($strength) {
         5 { $strengthMessage = "Erittäin vahva" }
         4 { $strengthMessage = "Vahva" }
         3 { $strengthMessage = "Kohtalainen" }
         2 { $strengthMessage = "Heikko" }
         default { $strengthMessage = "Erittäin heikko" }
     }
     return @{
         Password = $password
         Strength = $strengthMessage
         Tips = $strengthTips
     }
 }
 $password = Read-Host "Anna salasana, jonka haluat tarkistaa"

 $result = Test-PasswordStrength -password $password

 Write-Output "Salasanan vahvuus: $($result.Strength)"
 if ($result.Tips.Count -gt 0) {
     Write-Output "Vinkkejä salasanan parantamiseksi:"
     $result.Tips | ForEach-Object { Write-Output "- $_" }
 }
