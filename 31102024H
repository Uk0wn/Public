# Imposta la connessione al dominio
$domain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$root = ($domain.GetDirectoryEntry())

# Crea il DirectorySearcher
$searcher = New-Object System.DirectoryServices.DirectorySearcher
$searcher.SearchRoot = $root
$searcher.Filter = "(&(objectCategory=computer)(lastLogonTimestamp>= $([DateTime]::Now.AddDays(-30).ToFileTime())))" # Filtra per computer connessi negli ultimi 30 giorni
$searcher.PropertiesToLoad.Add("name") | Out-Null
$searcher.PropertiesToLoad.Add("operatingSystem") | Out-Null
$searcher.PropertiesToLoad.Add("lastLogonTimestamp") | Out-Null

# Esegui la ricerca
$results = $searcher.FindAll()

# Elabora e mostra i risultati
foreach ($result in $results) {
    $computer = $result.Properties
    [PSCustomObject]@{
        Name             = $computer.name[0]
        OperatingSystem  = $computer.operatingsystem[0]
        LastLogonDate    = [DateTime]::FromFileTime([Int64]$computer.lastLogonTimestamp[0])
    }
}
