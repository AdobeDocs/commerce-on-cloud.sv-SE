---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Inkludera fil för snabb ändring av anpassad VCL

## Ändra det anpassade VCL-fragmentet

1. [Logga in](/help/get-started/onboarding.md#access-your-admin-panel) i administratören.

1. Klicka på **Lagrar** > **Inställningar** > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Anpassade VCL-kodfragment**.

   ![Hantera anpassade VCL-fragment](/help/assets/cdn/fastly-manage-snippets.png)

1. Klicka på inställningsikonen bredvid det fragment som du vill redigera i kolumnen _Åtgärd_ .

1. När sidan har lästs in på nytt klickar du på **Överför VCL till Snabbt** i avsnittet _Snabbkonfiguration_.

1. När överföringen är klar uppdaterar du cacheminnet enligt meddelandet längst upp på sidan.

>[!WARNING]
>
>Gränssnittsalternativet _Anpassade VCL-fragment_ visar bara de fragment som lagts till via Adobe Commerce Admin. Om du lägger till fragment med API:t Snabb använder du API:t för att [hantera dem](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
