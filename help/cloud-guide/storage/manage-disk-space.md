---
title: Hantera diskutrymme
description: Lär dig hur du hanterar diskutrymme med hjälp av kommandoradsgränssnittet.
feature: Cloud, Storage
exl-id: 1d13dc4e-56eb-4153-a8b1-48d2263ebc4c
source-git-commit: b8cabaad4b7805858563cecbe5ffc2fdb9aeac58
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Hantera diskutrymme

Du kan hitta den totala lagringskapaciteten för ditt Cloud-projekt i ditt Adobe Commerce-avtal för molninfrastruktur och på din [kontosida](https://accounts.magento.cloud/user). Varje projektkort i ditt konto visar antalet _miljöer_, kapaciteten för _lagring_ i GB och antalet _användare_. Du kan också använda följande molnkommando:

```bash
magento-cloud subscription:info | grep storage
```

Exempelsvar:

```
| storage              | 51200
```

När en Pro-produktion eller staging-miljö når eller överskrider 95 % av lagringskapaciteten utlöser verktyget för övervakning av molninfrastruktur en supportvarning som meddelar dig om en automatisk ökning av lagringskapaciteten.

Exempelmeddelande:

>[!BEGINSHADEBOX]

_&quot;Vår övervakning har upptäckt att fillagringen på ditt kluster (projekt-id-miljö) börjar bli full. Diskanvändningen är för närvarande på en kritisk användningsnivå med mindre än 1 GiB kvar. Den delade lagringsvolymen håller på att uppgraderas från 60 GiB till 70 GiB för att hålla tjänsterna igång. Ta en titt på hur produktions- och mellanlagringsfilerna används för att se om du kan frigöra utrymme.&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Adobe rekommenderar att du regelbundet övervakar din lagringskapacitet och underhåller den väl under 90 % för att undvika dessa automatiska ökningar. När lagringsutrymmet för Pro-testning och -produktion har tilldelats är det permanent och kan inte återställas.

## Kontrollera integreringsmiljön

Du kan kontrollera hur mycket diskutrymme som används i integreringsmiljön med hjälp av CLI-programmet `magento-cloud`.

**Så här kontrollerar du ungefärlig diskutrymmesanvändning**:

```bash
magento-cloud db:size
```

Exempelsvar:

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Alla monteringar delar en skiva. Du kan kontrollera diskutrymmesanvändningen för monteringar med CLI:n för `magento-cloud`.

**Så här kontrollerar du ungefärlig diskutrymmesanvändning för mängder**:

```bash
magento-cloud mount:size
```

Exempelsvar:

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## Kontrollera dedikerade kluster

För Pro-miljöer för mellanlagring och produktion kan du kontrollera hur mycket diskutrymme som används i varje miljö med kommandot `disk free` som anger hur mycket diskutrymme som används i filsystemet. Du måste använda SSH för att logga in i en fjärrmiljö.

```bash
df -h
```

Alternativet `-h` visar rapporten i ett läsbart format (KB, MB eller GB).

I följande exempelsvar visar monteringen `/data/exports` diskutrymmet för media och monteringen `/data/mysql/` visar diskutrymme för databasen:

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

Du kan begränsa svaret genom att ange en katalog. Exempel:

```bash
df -h var/
```

Exempelsvar:

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Allokera diskutrymme

Två [konfigurationsfiler](../environment/overview.md) styr tilldelningen av diskutrymme i molnmiljöer: filen `.magento.app.yaml` och filen `.magento/services.yaml`. Varje fil innehåller egenskapen `disk` som definierar värdet för diskstorleken i MB för respektive konfiguration. Du kan bara ändra diskutrymme för Pro-integrering och Starter-miljöer.

>[!IMPORTANT]
>
>För Pro Production- och mellanlagringsmiljöer måste du [skicka in en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) om du vill ändra diskutrymme för allokering. En storleksökning av Pro Production- och Staging-miljöerna kan endast utföras med vissa intervall, så beroende på hur mycket diskutrymme du använder kan stödet rekommendera att du ökar diskutrymmet med minst 10 GB. Lagringsökningen för Pro-testning och -produktion kan inte återställas när den har tilldelats. Lagring kan inte omfördelas eller omfördelas mellan resurser. Minska diskutrymmet som tilldelats MySQL om du vill lägga till mer lagringsutrymme.

### Programdiskutrymme

Filen `.magento.app.yaml` styr det [beständiga diskutrymme](../application/properties.md#disk) som är tillgängligt för programmet.

**Så här ökar du diskutrymmet för programmet**:

1. Öppna konfigurationsfilen `.magento.app.yaml` i den lokala utvecklingsmiljön.

1. Ange ett nytt värde för egenskapen `disk` (i MB).

   ```yaml
   disk: <value-mb>
   ```

1. Spara ändringarna i filen.

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   Ändringarna börjar gälla när du har överfört den uppdaterade YAML-filen till fjärrmiljön.

### Tjänstdiskutrymme

Filen `.magento/services.yaml` styr vilket diskutrymme som är tillgängligt för varje tjänst, till exempel MySQL och Redis.

**Så här ökar du diskutrymmet för en tjänst**:

1. Öppna konfigurationsfilen `.magento/services.yaml` i den lokala utvecklingsmiljön.

1. Lägg till eller hitta en tjänst i filen. Se [mer om hur du konfigurerar tjänster](../services/services-yaml.md).

1. Ange ett nytt värde för diskegenskapen (i MB).

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. Spara ändringarna i filen.

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   Ändringarna börjar gälla när du har överfört den uppdaterade YAML-filen till fjärrmiljön.

## Övervaka diskutrymme

I Pro Production-miljöer kan du övervaka diskutrymme och andra prestandaindikatorer med hjälp av varningspolicyn för Adobe Commerce för New Relic. Mer information finns i [Övervaka prestanda med hanterade aviseringar](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Mer vägledning finns i [Bästa tillvägagångssätt för att lösa databasprestandaproblem](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html?lang=sv-SE).

## Inget utrymme kvar

Build-cachen kan växa med tiden. Om du får en varning om att tillstånden `No space left on device` kan du försöka rensa byggcachen och omdistribuera:

```bash
magento-cloud project:clear-build-cache
```
