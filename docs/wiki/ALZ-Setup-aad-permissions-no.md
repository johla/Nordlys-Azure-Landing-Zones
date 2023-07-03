# Konfigurer Azure Active Directory-tilganger for Service Principals

Denne artikkelen vil guide deg gjennom prosessen med å legge til din AzOps-Service Principal i Azure Active Directory [Directory Readers](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-manage) rolle.

> Merk: Trinnene nedenfor krever at du bruker en identitet som er lokal for Azure AD, og **ikke** en gjestebrukerkonto på grunn av kjente begrensninger.

Service Principals som brukes av Nordlys arkitekturens referanseimplementering krever Azure AD-directorylesertilganger for å kunne oppdage Azure-rolletildelinger. Disse tillatelsene brukes til å berike dataene om rolletildelinger med tilleggsinformasjon om Azure AD, som ObjectType og Azure AD Object DisplayName.

## Legg til Service Principals i katalogrolle via Azure Portal (alternativ 1)

1.1 Logg inn på Azure-portalen eller Azure Active Directory-adminsentret som en global administrator. Hvis du bruker Azure AD Privileged Identity Management, aktiver rolletildelingen for global administrator.

1.2 Åpne Azure Active Directory.

1.3 Under _Administrasjon_ > _Roller og administratorer_, velg _Directory-lesere_.
![alt](./media/aad-rolesandadministrators.png)

1.4 Under _Administrasjon_ > _Tildelinger_ > _Legg til tildelinger_, finn og velg AzOps-ServicePrincipal'en din, og legg den til i katalogrollen.

![alt](./media/directory-reader.png)

> Merk: Hvis du bruker Azure AD Privileged Identity Management, må du sørge for å legge til Service Principals i rollen med en permanent tildeling.

## Legg til tjenestepal i katalogrolle med Azure AD PowerShell (alternativ 2)

Sørg for at du har [AzureAD PowerShell-modulen installert på maskinen din](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) og at du har koblet til Azure AD med [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdleten.

````powershell
#Param -- Standard er AZOps
$ADServicePrincipal = "AZOps"

#Sjekk om AzureAD-modulen er installert og kjører en minimumsversjon, hvis ikke installer med nyeste versjon.
if (-not (Get-InstalledModule -Name "AzureAD" -MinimumVersion 2.0.2.130 ` -ErrorAction 'SilentlyContinue')) {

    Write-Host "AzureAD-modulen finnes ikke" -ForegroundColor 'Yellow'
    Install-Module -Name 'AzureAD' -Force
}
else {
    Write-Host "AzureAD-modulen finnes med minimumsversjon" -ForegroundColor 'Yellow'
}
Connect-AzureAD #Logg på Azure fra Powershell, dette vil omdirigere deg til en nettleser for autentisering, hvis nødvendig

#Bekreft tjenestepalen og velg en ny hvis den ikke finnes.
if (-not (Get-AzureADServicePrincipal -Filter "DisplayName eq '$ADServicePrincipal'")) { 
    Write-Host

 "Tjenestepal eksisterer ikke eller er ikke AZOps" -ForegroundColor 'Red'
    break
}
else { 
    Write-Host "$ADServicePrincipal eksisterer" -ForegroundColor 'Green'
    $ServicePrincipal = Get-AzureADServicePrincipal -Filter "DisplayName eq '$ADServicePrincipal'"
    #Få Azure AD Directory-rolle
    $DirectoryRole = Get-AzureADDirectoryRole -Filter "DisplayName eq 'Directory-lesere'"
    #Legg til tjenestepal i Directory-rolle
    Add-AzureADDirectoryRoleMember -ObjectId $DirectoryRole.ObjectId -RefObjectId $ServicePrincipal.ObjectId
}
````

Vær oppmerksom på at det kan ta opptil 15-30 minutter for tillatelsen skal spre seg i Azure AD.

## Neste trinn

Fortsett med [implementering av referanseimplementasjonen](./ALZ-Deploy-reference-implementations-no).