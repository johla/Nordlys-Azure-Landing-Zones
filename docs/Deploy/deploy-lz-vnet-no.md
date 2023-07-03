# Driftsetting av VNet-distribusjoner for Nordlys arkitekturen

Denne artikkelen beskriver hvordan man administrerer driftsetting av VNet-distribusjoner i Landing Zone for Enterprise-Scale miljøer ved bruk av policy-basert styring.

## Innledning

For å fullt ut omfavne abonnementsdemokratisering og autonome team i en Nordlys arkitektur, må vi aktivere automatiserte prosesser for å driftsette våre landingssoner. Denne prosessen, ofte omtalt som "File -> New -> Landing Zone", omfatter de gjentagende aktivitetene som kreves for å opprette en ny landingssone.

I scenarioer der landingssonen er koblet til bedriftens nettverk (Corp connected landing zone), uavhengig av valgt [Azure-nettverkstopologi](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/network-topology-and-connectivity#define-an-azure-network-topology) (hub-spoke eller Virtual WAN), må det utføres en ekstra serie nettverksrelaterte driftsettelser for å sikre at landingssonen er klar til bruk for applikasjonsteamene, for eksempel:

- Driftsett en virtuell nettverk (VNet) i landingssonens abonnement.

- Koble landingssonens VNet til et hub-VNet eller en vWAN-virtuell hub via VNet-peering.

- Driftsett en nettverkssikkerhetsgruppe (NSG) med standard sikkerhetsregler.

- \[Kun hub-spoke\] Driftsett en brukerdefinert rute (UDR) med standardrute til brannmur/NVA.

Når disse nettverksressursene er driftsatt, vil bedriftens tilkoblede landingssoner ha transittforbindelse til lokalt nettverk (via ExpressRoute eller VPN), mellom landingssoner, samt internettutgående trafikk via en sentral Azure-brannmur eller NVA, slik det vises på illustrasjonene nedenfor:

![VWAN-tilkoblet](./media/vnet_image1.png)

***Figur 1: VWAN-basert, tilkoblet VNet for bedriftens bruk***

![Hub & Spoke-tilkoblet](./media/vnet_image2.png)

***Figur 2: Hub-spoke-basert, tilkoblet VNet for bedriftens bruk***

Som en del av våre referanseimplementeringer for Nordlys arkitekturen har vi inkludert eksempler på hvordan man administrerer disse driftssettelsene i stor skala ved hjelp av Azure Policy. Denne artikkelen beskriver hvordan disse policyene fungerer og hvordan de kan brukes til å operasjonalisere VNet-distribusjoner i Landing Zones.

Hvis du bare ønsker å komme i gang med å driftsette tilkoblingsabonnementet ditt og landingssonen din uten å gå inn i detaljene i policyene, anbefaler vi at du utforsker våre [referanseimplementeringer for Nordlys arkitekturen](https://github.com/azure/enterprise-scale#deploying-enterprise-scale-architecture-in-your-own-environment), som gir deg en "ett-klikks" opplevelse for å distribuere alt

 fra start til slutt.

## Azure Policy - Driftsetting av VNet i Landing Zone

Vi tilbyr for øyeblikket en policy for å driftsette VNets i landingssoner og koble dem til en tradisjonell VNet-hub. Denne policydefinisjonen (Deploy-VNET-HubSpoke) er en del av den større settet med policyer som tilbys som standard i malen som finnes [her](https://github.com/Azure/Enterprise-Scale/blob/main/eslzArm/managementGroupTemplates/policyDefinitions/policies.json#L878).

### Deploy-VNet-HubSpoke - Tilordning på abonnementsnivå

Nedenfor er en overordnet fremgangsmåte for å opprette Landing Zone VNets koblet til tilkoblingshubben ved hjelp av policy. Denne artikkelen vil dekke de fremhevede trinnene.

Opprettelsen av abonnementet dekkes i [denne dokumentasjonen](https://github.com/Azure/Enterprise-Scale/wiki/Create-Landingzones).

![Deploy Hub & Spoke](./media/vnet_image3.png)

1. Tilordne policy til landingssone/abonnement

    **a)** Finn følgende policy og tilordne den til det nylig opprettede abonnementet.

    ![Tilordne policy](./media/vnet_image4.png)

    **b)** Angi alle nødvendige parametere og juster innstillingene for [GatewayTransit og UseRemoteGateway](https://learn.microsoft.com/azure/vpn-gateway/vpn-gateway-peering-gateway-transit#:~:text=In%20the%20Azure%20portal%2C%20navigate,Peerings%2C%20then%20select%20%2B%20Add.&text=Verify%20the%20subscription%20is%20correct,the%20Hub%2DRM%20virtual%20network.) hvis du har en VPN eller ExpressRoute-gateway som du planlegger å bruke for tilkobling til lokalt nettverk i hub-nettverket.

    > **Viktig:** Sørg for at CIDR-rekkevidden som angis ikke blir brukt andre steder i nettverket ditt.

    ![Tilordne policy](./media/vnet_image5.png)

    **c)** Endre "Managed Identity location" for MI (Managed Identity) som vil bli brukt for påfølgende driftsettelser til ønsket region, og opprett deretter tilordningen.

    ![Tilordne policy](./media/vnet_image6.png)

    ![Tilordne policy](./media/vnet_image7.png)

    **PowerShell**

    ```powershell
    # Hent policydefinisjonen som skal tilordnes
    $PolicyDefinition = Get-AzPolicyDefinition -Id /providers/Microsoft.Management/managementGroups/Corp/providers/Microsoft.Authorization/policyDefinitions/Deploy-VNET-HubSpoke

    # Hent abonnementet der policyen skal tilordnes for å tilordne policyen
    $Subscription = Get-AzSubscription -SubscriptionName "LZ2"

    # Opprett parameterobjektet for tilordningen
    $ParametersJson = @"
    {
        "vNetName": {
          "value": "LZ2-VNet"
        },
        "vNetRgName": {
          "value": "LZ2-Network"
        },
        "vNetLocation": {


          "value": "westeurope"
        },
        "vNetCidrRange": {
          "value": "10.243.0.0/24"
        },
        "hubResourceId": {
          "value": "/subscriptions/d1411a38-ff6c-4daf-8008-1d165bb753c9/resourceGroups/365-hub/providers/Microsoft.Network/virtualNetworks/365-hub-weu"
        },
        "hubPeerAllowGatewayTransit": {
          "value": true
        },
        "lzVnetUseRemoteGateway": {
          "value": false
        }
      }
    "@

    # Tilordne policyen til abonnementet
    $Assignment = New-AzPolicyAssignment -Name 'Deploy-VNet-HubSpoke-LZ2' `
                                       -DisplayName 'Deploy-VNet-HubSpoke-LZ2' `
                                       -Scope "/subscriptions/$($Subscription.SubscriptionId)" `
                                       -PolicyDefinition $PolicyDefinition `
                                       -PolicyParameter $ParametersJson `
                                       -AssignIdentity `
                                       -Location westeurope

    # Opprett nødvendig rolletilordning for policytilordningen
    New-AzRoleAssignment -RoleDefinitionId "$($PolicyDefinition.Properties.PolicyRule.then.details.roleDefinitionIds.split("/")[-1])" `
                         -ObjectId $assignment.Identity.principalId -Scope $Assignment.Properties.Scope
    ```

2. Opprett rolletilordning for administrert identitet i tilkoblingsabonnementet

    For å kunne opprette peering fra hub-siden, trenger policytilordningen for administrert identitet "Contributor"-tillatelser i tilkoblingsabonnementet.

    **a)** Rediger den tidligere opprettede policytilordningen og kopier navnet på policytilordningen. Hvis tilordningen ble utført via portalen, vil den ha et automatisk generert navn som vist i skjermbildet nedenfor.
    ![Rolletilordning](./media/vnet_image8.png)

    **b)** I Azure-portalen, naviger til tilkoblingsabonnementet og tilordne rollen "Contributor" til den administrerte identiteten.

    ![Rolletilordning](./media/vnet_image9.png)

    **PowerShell**

    ```powershell
    # Hent HubResourceId fra Parameters Json
    $HubResourceId = ($ParametersJson | ConvertFrom-Json).hubresourceId.value
    # Tilordne rolle
    New-AzRoleAssignment -RoleDefinitionName "Network Contributor" `
                         -ObjectId $assignment.Identity.principalId -Scope $HubResourceId
    ```

3. Resolve av policy for å starte distribusjonen

    **a)** Under Policy -> Resolve i portalen, finn og velg den tidligere opprettede tilordningen.

    ![Resolve](./media/vnet_image10.png)

    **b)** Klikk på "Resolve" for å starte oppgaven med å distribuere VNet og hub-peeringen. Det tar vanligvis 2-3 minutter, og du kan følge fremdriften under "Resolvesoppgaver".

    ![Resolve](./media/vnet_image11.png)
    ![Resolve](./media/vnet_image12.png)

    ![Resolve](./media/vnet_image13.png)

    **PowerShell**

    ```powershell
    # Start Resolve av policy
    Start-AzPolicyRemediation -PolicyAssignmentId $Assignment.P

olicyAssignmentId -Name $Assignment.Name
    # Hent Resolvesstatus
    Get-AzPolicyRemediation -Name $Assignment.Name
    ```