#============================================================================
#	Datei:		Test_PowerBI_Streaming.ps1
#
#	Summary:	Dieses Script erzeugt einen Datenstrom für Power BI der an
#               den angegebenen Endpunkt gesendet wird. Damit das funktioniert
#               müssen Sie zunächst, so wie im Buch "Power BI für Dummies"
#               beschrieben, ein Streamin Dataset einrichten. Dabei wird für
#               Ihr Setup ein eindeutiger Endpunkt erzeugt. Diesen Endpunkt
#               fügen Sie unten an Stelle von <IhrEndpunkt> ein.
#
#	Datum:		2019-07-22
#
#	Projekt:	Power BI für Dummies
#
#	PowerShell Version: 5.1
#------------------------------------------------------------------------------
#	Geschrieben von 
#       Frank Geisler, GDS Business Intelligence GmbH
#
#   Dieses Script ist nur zu Lehr- bzw. Lernzwecken gedacht
#
#   DIESER CODE UND DIE ENTHALTENEN INFORMATIONEN WERDEN OHNE GEWÄHR JEGLICHER
#   ART ZUR VERFÜGUNG GESTELLT, WEDER AUSDRÜCKLICH NOCH IMPLIZIT, EINSCHLIESSLICH,
#   ABER NICHT BESCHRÄNKT AUF FUNKTIONALITÄT ODER EIGNUNG FÜR EINEN BESTIMMTEN
#   ZWECK. SIE VERWENDEN DEN CODE AUF EIGENE GEFAHR.
#============================================================================*/

$endpoint = "<IhrEndpunkt>"

# Der Stream soll 10 Minuten lang laufen
$timeout = New-TimeSpan `
            -Minutes 10

$sw = [diagnostics.stopwatch]::StartNew()

while ($sw.elapsed -lt $timeout) {
    # Es wird zufällig eine Zahl zwischen 0 und 100 erzeugt
    $zahl = Get-Random `
                -Maximum 100

    # Aufbau des Payload-Objekts das unten in JSON umgewandelt wird
    $payload = @{
        "Kategorie" ="AAAAA555555"
        "Nummer" = $zahl
        }

    # Wert wird an den Power BI Service geschickt
    Invoke-RestMethod `
        -Method Post `
        -Uri "$endpoint" `
        -Body (ConvertTo-Json @($payload))

    Write-Host "Die aktuelle Zahl ist: $zahl"
    
    # Eine Sekunde warten, damit wir den Power BI Service nicht
    # mit Daten fluten
    Start-Sleep `
        -Seconds 1
}