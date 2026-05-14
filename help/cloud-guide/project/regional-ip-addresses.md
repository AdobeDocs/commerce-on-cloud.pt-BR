---
title: Endereços IP regionais
description: Consulte uma lista de endereços IP para as regiões do AWS e do Azure usadas pelo Adobe Commerce na infraestrutura em nuvem para ambientes de integração.
exl-id: 1137f5cf-4879-46d7-878c-bf47de7a0e34
source-git-commit: 84f9a4ba4d3942fc0461b18aeda2405ab2fbf67e
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Endereços IP regionais

As tabelas a seguir listam os endereços IP de entrada e saída usados pelo Adobe Commerce em [ambientes de integração](../architecture/pro-architecture.md#integration-environment) da infraestrutura em nuvem. Esses endereços IP são estáveis, mas podem mudar. A Adobe notifica os clientes antes de fazer qualquer alteração no endereço IP.

A sintaxe para lidar com os ambientes de integração é a seguinte:

```text
<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud
```

- **Identificação exclusiva** = 7 caracteres alfanuméricos aleatórios
- **ID do projeto** = ID do projeto com 13 caracteres
- **Região** = nome da região do AWS ou Azure

Você pode usar o comando `ping` ou `dig` para recuperar o endereço IP de entrada:

**Ping**

```bash
ping integration-abcd123-abcd78910.us-3.magentosite.cloud
```

Exemplo de resposta:

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

Exemplo de resposta

```bash
34.210.133.187
```

Se você tiver um firewall corporativo que bloqueia conexões SSH de saída, poderá adicionar os endereços IP de entrada ao seu incluo na lista de permissões.

## Regiões do AWS

|     | Estados Unidos |       |      | Europa |      |      |      | Ásia-Pacífico |
| --- | :------------ | :---- | :--- | :----- | :--- | :--- | :--- | :----------- |
|     | EUA | US-3 | US-5 | UE | UE-3 | UE-5 | UE-6 | AP-3 |
| Entrada | 52.200.159.23<p>52.200.159.125<p>52.200.160.5 | 34.210.133.187<p>34.214.72.239<p>34.215.10.85 | 50.112.160.58<p>54.213.195.223<p>35.163.170.185 | 52.209.44.44<p>52.209.23.96<p>52.51.117.101 | 34.240.75.192<p>34.251.110.37<p>52.19.113.35 | 35.157.81.88<p>3.122.198.131<p>52.28.102.195 | 35.181.23.47<p>35.181.24.165<p>35.180.237.48 | 52.65.39.201<p>52.65.10.202<p>52.65.30.37 |
| Saída | 52.200.155.111<p>52.200.149.44<p>50.17.163.75 | 34.210.166.180<p>34.215.83.92<p>34.213.20.158 | 54.70.238.217<p>52.88.113.98<p>52.36.188.230 | 52.51.163.159<p>52.209.44.60<p>52.208.156.247 | 34.240.57.142<p>52.16.140.48<p>52.209.134.55 | 3.121.163.221<p>3.121.79.229<p>18.197.3.230 | 52.47.155.26<p>35.181.0.157<p>35.181.12.15 | 52.65.143.178<p>13.54.80.197<p>52.62.224.4 |

<!--US-->
<!--US-3-->
<!--US-5-->
<!--EU-->
<!--EU-3-->
<!--EU-5-->
<!--EU-6-->
<!--AP-3-->
<!--EU-->
<!--EU-3-->
<!--EU-5-->
<!--EU-6-->
<!--AP-3-->
<!--US-->
<!--US-3-->
<!--US-5-->




## Regiões do Azure

|          | Estados Unidos | Europa |
| -------- | :-------------- | :-------------- |
|          | US-A1 | AZ-WESTEUROPE-1 |
| Entrada | 20.186.27.68<p>20.186.58.163<p>20.186.113.8 | 50.112.160.58<p>54.213.195.223<p>35.163.170.185 |
| Saída | 20.186.58.163<p>20.186.27.68<p>20.186.113.8 | 104.45.78.98<p>51.105.168.218<p>51.105.163.143 |

<!--AZ-W-1-->
<!--US-A1-->
<!--US-A1-->
<!--AZ-W-1-->
