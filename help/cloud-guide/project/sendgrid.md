---
title: SkickaGrid-e-posttjänst
description: Lär dig mer om e-posttjänsten SendGrid för Adobe Commerce i molninfrastrukturen och hur du kan testa din DNS-konfiguration.
exl-id: 06236068-df32-468f-99ec-c379984be136
source-git-commit: 0cb86dd5e4fe627b198ac3c1a6b14607f377a9a3
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 0%

---

# SkickaGrid-e-posttjänst

Proxytjänsten SendGrid Simple Mail Transfer Protocol (SMTP) tillhandahåller utgående e-postautentisering och anseendeövervakning, inklusive stöd för:

* Alla utgående transaktionsmejl
* Dedikerade IP-adresser
* Domänregistrering, DKIM-signaturer (DomainKeys Identified Mail) för e-postdomänvalidering (endast för Pro Staging- och Production-miljöer)
* Anpassad domänregistrering (endast för Pro)
* Automatisk integrering för integreringsmiljöerna Starter och Pro. Proffsproduktions- och mellanlagringsmiljöer kräver manuell etablering och konfiguration under maskinvaruprovisioneringsprocessen för Infrastructure as a Service (IaaS)

SendGrid SMTP-proxyn är inte avsedd att användas som en allmän e-postserver för att ta emot inkommande e-post eller för användning med e-postmarknadsföringskampanjer. Om du planerar att importera och skicka välkomstmeddelanden till ett stort antal kunder (till exempel 10 000 eller fler) ska du dela upp importen och e-postmeddelandena på flera dagar. Den här metoden hjälper dig att hålla dig inom de gränser som gäller för dagliga sändningar och skyddar ditt avsändarrykte.

>[!TIP]
>
>Kontrollera att du har konfigurerat rätt e-postadresser för butik i administratören genom att gå till Lager > Konfiguration > Allmänt för att undvika problem med levererbarhet och domänverifiering. Du måste avmarkera **[!UICONTROL Use Default]** och ersätta standardvärdena med en domän som du äger. E-posttjänster för offentlig/delad domän, som gmail.com och outlook.com, ska inte konfigureras som avsändarens e-postadress när e-postmeddelanden skickas via Sendgrid.

## Aktivera eller inaktivera e-post

Du kan aktivera eller inaktivera utgående e-post för varje miljö från molnkonsolen eller från kommandoraden.

Som standard är utgående e-post aktiverat i Pro Production- och Staging-miljöer. [!UICONTROL Outgoing emails] kan dock visas som inaktiverat i miljöinställningarna tills du ställer in egenskapen `enable_smtp` via [kommandoraden](outgoing-emails.md#enable-emails-in-the-cli) eller [molnkonsolen](outgoing-emails.md#enable-emails-in-the-cloud-console). Du kan aktivera utgående e-postmeddelanden för integrerings- och staging-miljöer för att skicka tvåfaktorsautentisering eller återställa e-postmeddelanden med lösenord för användare av Cloud-projekt. Se [Konfigurera e-postmeddelanden för testning](outgoing-emails.md).

Om utgående e-post måste inaktiveras eller återaktiveras i Pro Production- eller Staging-miljöer kan du skicka en [Adobe Commerce Support-biljett](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Om du uppdaterar egenskapsvärdet [!UICONTROL enable_smtp] med [kommandorad](outgoing-emails.md#enable-emails-in-the-cli) ändras även inställningsvärdet [!UICONTROL Enable outgoing emails] för den här miljön på [molnkonsolen](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Kontrollpanel för SendGrid

Alla Cloud-projekt hanteras under ett centralt konto, så bara supporten har tillgång till kontrollpanelen SendGrid. SendGrid innehåller inga funktioner för begränsning av underkonto.

Om du vill granska aktivitetsloggarna för leveransstatus eller en lista över e-postadresser som har studsats, avvisats eller blockerats, [skickar du en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). Supportteamet **kan inte** hämta aktivitetsloggar som är äldre än 30 dagar.

Om det är möjligt kan du inkludera följande information i din begäran:

* e-postadresser som påverkas
* den aktuella tidsramen (endast under de senaste 30 dagarna)
* e-postmeddelandets ämne

## Identifierad e-post för DomainKeys (DKIM)

DKIM är en teknik för e-postautentisering som gör det möjligt för Internet-leverantörer (ISP) att identifiera både giltiga och falska avsändaradresser, en teknik som ofta används vid nätfiske och e-postbedrägerier. DKIM förlitar sig på en domänägare som hanterar DNS-posterna. När du använder DKIM använder avsändarservern en privat nyckel för att signera meddelandena. Domänägaren lägger också till en DKIM-post, som är en ändrad `TXT`-post, i DNS-posterna för avsändardomänen. Den här `TXT`-posten innehåller en offentlig nyckel som mottagarens e-postservrar använder för att verifiera signaturen för ett meddelande. Med DKIM kryptografiprocedur för offentlig nyckel kan mottagarna verifiera en avsändares äkthet. Se [DKIM Records Expected](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Stöd för DKIM-signaturer i SendGrid och domänautentisering är endast tillgängligt i produktions- och mellanlagringsmiljöerna för Pro-projekt, men inte för alla Starter-miljöer. Därför är det troligt att utgående transaktionsmejl flaggas av skräppostfilter. Med DKIM förbättras leveransfrekvensen som en autentiserad e-postavsändare. Om du vill förbättra leveransfrekvensen för meddelanden kan du uppgradera från Starter till Pro eller använda en egen SMTP-server eller e-postleverantör. Se [Konfigurera e-postanslutningar](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications) i handboken _Administratörssystem_.

### Avsändare och domänautentisering

För att SendGrid ska kunna skicka transaktionsmeddelanden för din räkning från Pro Production- eller Staging-miljöer måste du konfigurera dina DNS-inställningar så att de inkluderar de tre DNS-posterna för SendGrid-underdomänen. Varje SendGrid-konto tilldelas en unik `TXT`-post som används för att autentisera utgående e-postmeddelanden.

>[!TIP]
>
>Kontrollera att du har konfigurerat **[!UICONTROLSStore-e-postadresserna]** med rätt domän i **[!UICONTROL Stores > Configuration > General > Store Email Addresses]**. Domänautentiseringen utförs på avsändarens e-postadress. Om standardinställningen (`example.com`) är konfigurerad blockeras e-postmeddelanden från `example.com` av Sendgrid.

**Så här aktiverar du domänautentisering**:

1. Skicka en [supportanmälan](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) för att begära aktivering av DKIM för en specifik domän (**Proffsmiljö, endast för mellanlagrings- och produktionsmiljöer**).
1. Uppdatera din DNS-konfiguration med de `TXT`- och `CNAME`-poster som du har fått i supportbiljetten.

**Exempel på `TXT`-post med konto-ID**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Exempel `CNAME` poster**:

| Domän | Punkter till | Posttyp |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM signaturer och automatiserad säkerhet

Du kan välja mellan automatiserad och manuell säkerhet när du skapar en autentiserad domän. Om du väljer automatiserad säkerhet hanterar SendGrid dina DKIM- och SPF-poster automatiskt. När du lägger till en ny dedikerad sändande IP-adress till ditt konto, uppdaterar SendGrid dina DNS-inställningar och DKIM-signaturer omedelbart. Om du stänger av automatisk säkerhet ansvarar du för att uppdatera din DKIM-signatur när som helst när du ändrar din sändande domän.

**Exempel på automatiserad säkerhet aktiverad**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Exempel på automatisk säkerhet inaktiverad**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

När domänautentisering har konfigurerats hanterar SendGrid automatiskt SPF (Security Policy Framework) och DKIM-poster åt dig. När SendGrid tillhandahåller de `CNAME`-poster som ska läggas till i dina DNS-poster kan du lägga till dedikerade IP-adresser och göra andra kontouppdateringar utan att behöva hantera dina SPF-poster manuellt. Se [Automatiserad säkerhet och din DKIM-signatur](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Så här testar du DNS-konfigurationen:

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Tröskelvärde för transaktionsmeddelanden

Tröskelvärdet för transaktionella e-postmeddelanden avser antalet transaktionsmeddelanden som du kan skicka från Pro-miljöer under en viss tidsperiod, till exempel 12 000 e-postmeddelanden per månad från icke-produktionsmiljöer. Tröskelvärdet är utformat för att skydda mot att skicka skräppost och eventuellt skada ditt e-postrykte.

Det finns inga strikta gränser för hur många e-postmeddelanden som kan skickas i produktionsmiljön, så länge poängen Sender Reputation är över 95 %. Anseendet påverkas av antalet avvisade eller avvisade e-postmeddelanden och om DNS-baserade skräppostregister har flaggat din domän som en potentiell skräppostkälla. Se [E-postmeddelanden som inte skickas när SendGrid-krediter överskrids på Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) i _Commerce Support Knowledge Base_.

**Så här kontrollerar du om det maximala antalet krediter har överskridits**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Kontrollera om `/var/log/mail.log` har `authentication failed : Maxium credits exceeded` poster.

   Om du ser `authentication failed` loggposter och om **e-postsändningsanseendet** är minst 95 kan du [skicka en Adobe Commerce Support-anmälan](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) och begära en ökning av kredittilldelningen.

>[!NOTE]
>
>Filen `var/log/mail.log` är en *loggfil som körs*. När nya poster läggs till tas äldre poster bort från filen över tid. Endast den senaste loggaktiviteten är tillgänglig i loggen. De äldre loggposterna arkiveras inte och sparas inte när de tagits bort från mail.log.

### E-postavsändarens rykte

Ett e-postrykte är en poäng som tilldelats av en Internet-leverantör (ISP) till ett företag som skickar e-postmeddelanden. Ju högre poäng, desto troligare är det att en Internet-leverantör levererar meddelanden till en mottagares inkorg. Om poängen ligger under en viss nivå kan Internet-leverantören dirigera meddelanden till mottagarnas skräppostmapp eller till och med avvisa meddelanden helt. Anseendepoängen bestäms av flera faktorer, som ett 30-dagars genomsnitt av dina IP-adresser, jämfört med andra IP-adresser och andelen skräppost. Se [8 olika sätt att kontrollera din e-postutskicksframställning](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).

### Lista över e-postinaktiveringar

En lista över e-postinaktiveringar är en lista över mottagare som e-postmeddelanden inte ska skickas till om det skulle skada ditt anseende och din leveransfrekvens. Det krävs enligt CAN-SPAM Act för att säkerställa att e-postavsändare har en metod för att avanmäla mottagare som avbeställt eller markerat e-post som skräppost. Supprestionslistan samlar även in e-postmeddelanden som studsar, blockeras eller är ogiltiga.

Om du inte vill att e-postmeddelanden ska skickas till skräppostmappen överhuvudtaget, ska du följa Sendgrid artikel best practices, [Why Are My Email Going to Spam?](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder).

Om vissa mottagare inte får dina e-postmeddelanden kan du [skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) för att begära en granskning av listorna och ta bort mottagarna om det behövs.

Mer information finns i [Vad är en undertryckningslista?](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
