---
title: Atualize o pacote ECE-Tools
description: Saiba como atualizar o pacote ECE-Tools para aproveitar as correções e os recursos mais recentes aplicados ao Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Upgrade
exl-id: acb4fd0d-6ffb-4094-8dd4-83bb7735d64f
TQID: 'https://experienceleague.adobe.com/WqcVa0BJN0ot6yg2unhzeEB8nkjKXM7o26tOwmrg5mU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: a8ae7a5a-6cdc-5922-bd0d-6feb44b04984
    internal-label: Upgrade
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%
---
# Atualize o pacote ECE-Tools

Uma atualização do pacote `ece-tools` também atualiza o outro [Conjunto de Ferramentas da Nuvem para pacotes do Commerce](../release-notes/cloud-tools-suite.md), que são dependências para `ece-tools`. Portanto, você deve usar uma versão do Adobe Commerce na infraestrutura em nuvem compatível com o pacote `ece-tools`.

{{ece-tools-package}}

**Pré-requisitos**:

- Antes de atualizar o `ece-tools`, reveja as [notas de versão do Conjunto de Ferramentas da Nuvem para Commerce](../release-notes/cloud-tools-suite.md).
- Se você estiver atualizando do `ece-tools` 2002.0.22 ou anterior para o 2002.1.0, revise as [alterações incompatíveis com versões anteriores](../release-notes/backward-incompatible-changes.md) e faça as alterações necessárias no projeto de infraestrutura do Adobe Commerce na nuvem.
- Revise [Atualizações e patches](../development/commerce-version.md#upgrade-from-older-versions) para determinar as versões das Ferramentas ECE compatíveis com seu projeto do Adobe Commerce na infraestrutura em nuvem.

{{upgrade-tip}}

**Para atualizar o `ece-tools` pacote**:

1. Na estação de trabalho local, execute uma atualização usando o Composer.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Se não for possível atualizar além da versão `ece-tools` 2002.0.8, consulte [Atualizar projeto para usar o pacote ECE-Tools](install-package.md).

1. Adicionar, confirmar e enviar alterações de código.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Após a validação do teste, mescle essa ramificação à ramificação de integração.
