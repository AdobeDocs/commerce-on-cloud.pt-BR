---
title: Manipuladores de log
description: Saiba como configurar manipuladores de log para o Adobe Commerce na infraestrutura da nuvem.
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Manipuladores de log

Você pode configurar manipuladores de log para enviar mensagens a um servidor de log remoto. Um manipulador de log envia registros de criação e implantação para outros sistemas, de forma semelhante à maneira como você envia registros para o Slack e email. Você pode habilitar um manipulador _syslog_, que é ideal para registrar mensagens relacionadas ao hardware, ou um manipulador GELF (Formato de Log Estendido) Graylog, que é ideal para registrar mensagens de aplicativos de software.

O exemplo a seguir configura esses dois manipuladores adicionando a configuração ao arquivo `.magento.env.yaml`. Para obter os valores de nível de log mínimo (`min_level`), consulte [Níveis de log](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Níveis de log

Os níveis de log determinam o nível de detalhes nas mensagens de notificação. As seguintes categorias de nível de log incluem todos os níveis de log abaixo dele. Por exemplo, um nível `debug` inclui logs de todos os níveis, enquanto um nível `alert` mostra apenas alertas e emergências.

- **depurar**—informações detalhadas sobre a depuração
- **info** — eventos interessantes, como logon de usuário ou log SQL
- **aviso**—eventos normais, mas significativos
- **aviso**—ocorrências excepcionais que não sejam erros, como o uso de uma API obsoleta ou o uso inadequado de uma API
- **erro**—erros de tempo de execução que não requerem ação imediata
- **crítico**—condições críticas, como um componente de aplicativo indisponível ou uma exceção inesperada
- **alerta**—ação imediata necessária—como um site estar desativado ou o banco de dados não estar disponível—que dispara um alerta de SMS
- **emergência**—o sistema não pode ser usado
