---
title: Regionala IP-adresser
description: Se en lista över IP-adresser för de AWS- och Azure-regioner som används av Adobe Commerce i molninfrastruktur för integreringsmiljöer.
exl-id: 1137f5cf-4879-46d7-878c-bf47de7a0e34
source-git-commit: f2214dd56625847132298892635c7cf738c3d71f
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---

# Regionala IP-adresser

I följande tabeller visas de inkommande och utgående IP-adresser som används av Adobe Commerce i [integreringsmiljöer](../architecture/pro-architecture.md#integration-environment) för molninfrastrukturen. Dessa IP-adresser är stabila, men kan komma att ändras. Adobe meddelar sina kunder innan de ändrar IP-adressen.

Följande syntax används för att hantera integreringsmiljöer:

```text
<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud
```

- **Unikt ID** = 7 slumpmässiga alfanumeriska tecken
- **Projekt-ID** = projekt-ID med 13 tecken
- **Region** = AWS- eller Azure-regionnamn

Du kan använda kommandot `ping` eller `dig` för att hämta den inkommande IP-adressen:

**Ping**

```bash
ping integration-abcd123-abcd78910.us-3.magentosite.cloud
```

Exempelsvar:

```console
PING integration-abcd123-abcd78910.us-3.magentosite.cloud (34.210.133.187): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
```

**Dig**

```bash
dig +short integration-abcd123-abcd78910.us-3.magentosite.cloud
```

Exempelsvar

```bash
34.210.133.187
```

Om du har en företagsbrandvägg som blockerar utgående SSH-anslutningar kan du lägga till inkommande IP-adresser i tillåtelselista.

## AWS

|     | Amerikas förenta stater |       |      | Europa |      |      |      | Asien-Stillahavsområdet |
| --- | :------------ | :---- | :--- | :----- | :--- | :--- | :--- | :----------- |
|     | USA | US-3 | US-5 | EU | EU-3 | EU-5 | EU-6 | AP-3 |
| Inkommande | <!--US-->52.200.159.23<p>52.200.159.125<p>52.200.160.5 | <!--US-3-->34.210.133.187<p>34.214.72.239<p>34.215.10.85 | <!--US-5-->50.112.160.58<p>54.213.195.223<p>35.163.170.185 | <!--EU-->52.209.44.44<p>52.209.23.96<p>52.51.117.101 | <!--EU-3-->34.240.75.192<p>34.251.110.37<p>52.19.113.35 | <!--EU-5-->35.157.81.88<p>3.122.198.131<p>52.28.102.195 | <!--EU-6-->35.181.23.47<p>35.181.24.165<p>35.180.237.48 | <!--AP-3-->52.65.39.201<p>52.65.10.202<p>52.65.30.37 |
| Utgående | <!--US-->52.200.155.111<p>52.200.149.44<p>50.17.163.75 | <!--US-3-->34.210.166.180<p>34.215.83.92<p>34.213.20.158 | <!--US-5-->54.70.238.217<p>52.88.113.98<p>52.36.188.230 | <!--EU-->52.51.163.159<p>52.209.44.60<p>52.208.156.247 | <!--EU-3-->34.240.57.142<p>52.16.140.48<p>52.209.134.55 | <!--EU-5-->3.121.163.221<p>3.121.79.229<p>18.197.3.230 | <!--EU-6-->52.47.155.26<p>35.181.0.157<p>35.181.12.15 | <!--AP-3-->52.65.143.178<p>13.54.80.197<p>52.62.224.4 |

## Azure-regioner

|          | Amerikas förenta stater | Europa |
| -------- | :-------------- | :-------------- |
|          | US-A1 | AZ-WESTEUROPE-1 |
| Inkommande | <!--US-A1--> 20.186.27.68<p>20.186.58.163<p>20.186.113.8 | <!--AZ-W-1-->50.112.160.58<p>54.213.195.223<p>35.163.170.185 |
| Utgående | <!--US-A1-->20.186.58.163<p>20.186.27.68<p>20.186.113.8 | <!--AZ-W-1-->104.45.78.98<p>51.105.168.218<p>51.105.163.143 |
