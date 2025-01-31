---
title: Gerenciamento de backup
description: Saiba como criar e restaurar manualmente um backup para seu projeto do Adobe Commerce na infraestrutura em nuvem.
feature: Cloud, Paas, Snapshots, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Gerenciamento de backup

Você pode executar um backup manual de ambientes Iniciadores ativos a qualquer momento usando o botão **[!UICONTROL Backup]** no [!DNL Cloud Console] ou usando o comando `magento-cloud snapshot:create`.

Um backup ou _instantâneo_ é um backup completo de dados do ambiente que inclui todos os dados persistentes de serviços em execução (banco de dados MySQL) e quaisquer arquivos armazenados nos volumes montados (var, pub/media, app/etc). O instantâneo _não_ inclui código, pois o código já está armazenado no repositório baseado em Git. Não é possível fazer download de uma cópia de um snapshot.

O recurso de backup/instantâneo **não** se aplica aos ambientes Pro Staging e Production, que recebem backups regulares para fins de recuperação de desastres por padrão. Consulte [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery) para obter mais informações. Ao contrário dos backups dinâmicos automáticos nos ambientes Pro Staging e Production, os backups são **não** automáticos. É _sua_ responsabilidade criar manualmente um backup ou configurar um trabalho cron para criar periodicamente um backup dos seus ambientes de integração Starter ou Pro.

## Criar um backup manual

Você pode criar um backup manual de qualquer ambiente Starter ativo e ambiente Pro de integração do [!DNL Cloud Console] ou criar um instantâneo da CLI da nuvem. Você deve ter uma [Função de administrador](../project/user-access.md) para o ambiente.

**Para criar um backup de qualquer ambiente Inicial usando o[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Selecione um ambiente na barra de navegação do projeto. O ambiente deve estar ativo.
1. Na exibição _Backups_, clique em **[!UICONTROL Backup]**. Essa opção não está disponível para um ambiente Pro.

   ![Backup](../../assets/button-backup.png){width="150"}

**Para criar um backup de um ambiente de integração usando o[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Selecione um ambiente de integração/desenvolvimento na barra de navegação do projeto. O ambiente deve estar ativo.
1. Selecione a opção **[!UICONTROL Backup]** no menu superior direito. Essa opção está disponível para ambientes Starter e Pro.
1. Clique no botão **[!UICONTROL Yes]**.

**Para criar um instantâneo usando a CLI** do `magento-cloud`:

1. Na estação de trabalho local, altere para o diretório do projeto.
1. Faça check-out da ramificação de ambiente para obter um instantâneo.
1. Crie o instantâneo.

   ```bash
   magento-cloud snapshot:create --live
   ```

   Como alternativa, você pode usar o comando short `magento-cloud backup`. A opção `--live` deixa o ambiente em execução para evitar tempo de inatividade. Para obter uma lista completa de opções, digite `magento-cloud snapshot:create --help`.

   Exemplo de resposta:

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Verifique os snapshots mais recentes.

   ```bash
   magento-cloud snapshot:list
   ```

   A lista retorna informações sobre o status do snapshot:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Restaurar um backup manual

Você deve ter [acesso de Administrador](../project/user-access.md) ao ambiente. Você tem até **sete dias** para _restaurar_ um backup manual. A restauração de um backup não altera o código da ramificação Git atual. Restaurar um backup dessa maneira não se aplica a ambientes de preparo e produção Pro; consulte [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Os tempos de restauração variam dependendo do tamanho do banco de dados:

- um banco de dados grande (mais de 200 GB) pode levar 5 horas
- banco de dados médio (150 GB) pode levar 2 horas e meia
- banco de dados pequeno (60 GB) pode levar 1 hora

>[!TIP]
>
>Restauração sem backup:
>
>- Para reverter para o código anterior ou remover extensões adicionadas em um ambiente, consulte [Reverter código](#roll-back-code).
>- Para restaurar um ambiente instável que _não_ tem um backup, consulte [Restaurar um ambiente](../development/restore-environment.md).

**Para restaurar um backup usando o[!DNL Cloud Console]**:

1. Faça logon no [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Selecione um ambiente na barra de navegação do projeto.
1. Na exibição _Backups_, escolha um backup na lista _Armazenados_. O recurso de backup **não** se aplica aos ambientes Pro.
1. No menu ![Mais](../../assets/icon-more.png){width="32"} (_mais_), clique em **Restaurar**.
1. Revise as informações de Restauração do backup e clique em **Sim, restaurar**.

**Para restaurar um instantâneo usando a CLI da Nuvem**:

1. Na estação de trabalho local, altere para o diretório do projeto.
1. Confira a ramificação do ambiente a ser restaurada.
1. Lista todos os snapshots disponíveis.

   ```bash
   magento-cloud snapshot:list
   ```

   A lista retorna informações sobre os snapshots disponíveis:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Restaure um instantâneo usando a ID de instantâneo da lista.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Restaurar um Instantâneo da Recuperação de Desastres

Para restaurar o Instantâneo da Recuperação de Desastres nos ambientes Pro Staging e Production, [Importe o despejo do banco de dados diretamente do servidor](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Reverter código

Backups e instantâneos _não_ incluem uma cópia do código. Seu código já está armazenado no repositório baseado em Git, portanto, você pode usar comandos baseados em Git para reverter (ou reverter) o código. Por exemplo, use `git log --oneline` para rolar pelas confirmações anteriores; em seguida, use [`git revert`](https://git-scm.com/docs/git-revert) para restaurar o código de uma confirmação específica.

Você também pode optar por armazenar o código em uma ramificação _inativa_. Use comandos git para criar uma ramificação em vez de usar comandos `magento-cloud`. Consulte sobre [comandos do Git](../dev-tools/cloud-cli-overview.md#git-commands) no tópico da CLI da nuvem.
