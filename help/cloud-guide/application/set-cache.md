---
title: Ange cache för statiska filer
description: Lär dig hur du anger cachelagringsalternativ i  [!DNL Commerce] programmets konfigurationsfil.
feature: Cloud, Configuration, Cache, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Ange cache för statiska filer

TTL-värdet för cache (time-to-live) för dina media och statiska filer anges i konfigurationsfilen `.magento.app.yaml` med nyckeln `expires`.

>[!NOTE]
>
>Innan du uppdaterar produktionsmiljön är det viktigt att du testar ändringarna i mellanlagringsmiljön. [Skicka in en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) om du behöver hjälp med att uppdatera konfigurationen i dessa miljöer.

1. Ange TTL-tiden (i sekunder) i [`web`-egenskapen ](web-property.md) för filen `.magento.app.yaml`. Du kan lägga till nyckeln `expires` under `locations` eller under `"/media"` och `"/static"`.

   Använd nyckelvärdepar `expires: -1` för att förhindra att cachen förfaller. Se följande exempel:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```
