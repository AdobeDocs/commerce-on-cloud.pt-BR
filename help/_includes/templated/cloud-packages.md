---
source-git-commit: 50e1a864dcde1148f5ed036b55815442c5986eaf
workflow-type: tm+mt
source-wordcount: '2313'
ht-degree: 0%

---
# Pacotes na nuvem do Adobe Commerce

<!-- The 'packages' variable contains the 'packages' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'packages-dev' variable contains the 'packages-dev' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'product' variable contains data of the 'magento/magento-cloud-metapackage' package
 -->

<!-- The edition variable contains `cloud` value from the _data/names.yml file
 -->

O Adobe Commerce na infraestrutura em nuvem usa o Composer para gerenciar pacotes PHP.

O arquivo `composer.json` declara a lista de pacotes, enquanto o arquivo `composer.lock` armazena uma lista completa dos pacotes (uma versão completa de cada pacote e suas dependências) usados para criar uma instalação do Adobe Commerce.

A documentação de referência a seguir é gerada a partir do arquivo `composer.lock` e abrange os pacotes necessários incluídos no Adobe Commerce na infraestrutura de nuvem 2.4.8-p1.

## Dependências

`magento/magento-cloud-metapackage 2.4.8-p1` tem as seguintes dependências:

```config
fastly/magento2: ^1.2.34
magento/ece-tools: ^2002.2.0
magento/module-paypal-on-boarding: ~100.5.0
magento/product-enterprise-edition: >=2.4.8 <2.4.9
```

## Licenças de terceiros

### Apache-2.0, somente LGPL-2.1

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/opensearch-project/opensearch-php.git">opensearch-project/opensearch-php</a>
    </td>
    <td>biblioteca</td>
    <td>Cliente PHP para OpenSearch</td>
  </tr>
  </tbody>
</table>

### Apache-2.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/adobe/stock-api-libphp.git">astock/stock-api-libphp</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de API do Adobe Stock</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/awslabs/aws-crt-php.git">aws/aws-crt-php</a>
    </td>
    <td>biblioteca</td>
    <td>AWS Common Runtime para PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/aws/aws-sdk-php.git">aws/aws-sdk-php</a>
    </td>
    <td>biblioteca</td>
    <td>AWS SDK para PHP - Use o Amazon Web Services no seu projeto PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opentelemetry-php/api.git">abrir-telemetria/api</a>
    </td>
    <td>biblioteca</td>
    <td>API para OpenTelemetry PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opentelemetry-php/context.git">abrir-telemetria/contexto</a>
    </td>
    <td>biblioteca</td>
    <td>Implementação de contexto para OpenTelemetry PHP.</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree
    </td>
    <td>metapackage</td>
    <td>Braintree Magento</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/stomp-php/stomp-php.git">stomp-php/stomp-php</a>
    </td>
    <td>biblioteca</td>
    <td>suporte a stomp para PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/wikimedia/less.php.git">wikimedia/less.php</a>
    </td>
    <td>biblioteca</td>
    <td>Porta PHP do processador LESS</td>
  </tr>
  </tbody>
</table>

### Cláusula BSD- 2

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/Bacon/BaconQrCode.git">bacon/bacon-qr-code</a>
    </td>
    <td>biblioteca</td>
    <td>BaconQrCode é um gerador de código QR para PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/DASPRiD/Enum.git">dasprid/enum</a>
    </td>
    <td>biblioteca</td>
    <td>Implementação do PHP 7.1 enum</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webimpress/safe-writer.git">webimpress/safe-writer</a>
    </td>
    <td>biblioteca</td>
    <td>Ferramenta para gravar arquivos com segurança, para evitar condições de corrida</td>
  </tr>
  </tbody>
</table>

### BSD-3-Cláusula

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_File.git">colinmollenhour/cache-backend-file</a>
    </td>
    <td>magento-module</td>
    <td>O back-end do estoque Zend_Cache_Backend_File tem desempenho extremamente baixo para limpeza por tags, tornando-o inutilizável à medida que o número de itens em cache aumenta. Esse back-end faz muitas alterações, resultando em um enorme aumento de desempenho, especialmente para a limpeza de tags.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/php-redis-session-abstract.git">colinmollenhour/php-redis-session-abstract</a>
    </td>
    <td>biblioteca</td>
    <td>Um gerenciador de sessões baseado no Redis com bloqueio otimista</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/duosecurity/duo_universal_php.git">duosecurity/duo_universal_php</a>
    </td>
    <td>biblioteca</td>
    <td>Uma implementação em PHP do Duo Universal SDK.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/fastly/fastly-magento2.git">fastly/magento2</a>
    </td>
    <td>magento2-module</td>
    <td>Módulo Fastly CDN para Magento 2.4.x</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/firebase/php-jwt.git">firebase/php-jwt</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca simples para codificar e decodificar JSON Web Tokens (JWT) no PHP. Deve estar em conformidade com as especificações atuais.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-captcha.git">laminas/laminas-captcha</a>
    </td>
    <td>biblioteca</td>
    <td>Gerar e validar CAPTCHAs usando Figlets, imagens, ReCaptcha e muito mais</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-code.git">laminas/laminas-code</a>
    </td>
    <td>biblioteca</td>
    <td>Extensões para a API de reflexão do PHP, varredura de código estático e geração de código</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-config.git">laminas/laminas-config</a>
    </td>
    <td>biblioteca</td>
    <td>O fornece uma interface do usuário baseada em propriedade de objeto aninhado para acessar esses dados de configuração no código do aplicativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-di.git">laminas/laminas-di</a>
    </td>
    <td>biblioteca</td>
    <td>Injeção de dependência automatizada para contêineres PSR-11</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-escaper.git">laminas/laminas-escaper</a>
    </td>
    <td>biblioteca</td>
    <td>Escapar com segurança do HTML, atributos do HTML, JavaScript, CSS e URLs</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-eventmanager.git">laminas/laminas-eventmanager</a>
    </td>
    <td>biblioteca</td>
    <td>Acionar e ouvir eventos em um aplicativo PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-feed.git">laminas/laminas-feed</a>
    </td>
    <td>biblioteca</td>
    <td>fornece funcionalidade para criar e consumir RSS e Atom feeds</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-filter.git">laminas/laminas-filter</a>
    </td>
    <td>biblioteca</td>
    <td>Filtrar e normalizar dados e arquivos de forma programática</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-http.git">laminas/laminas-http</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece uma interface fácil para executar solicitações do HTTP (Hyper-Text Transfer Protocol)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-i18n.git">laminas/laminas-i18n</a>
    </td>
    <td>biblioteca</td>
    <td>Fornecer traduções para seu aplicativo e filtrar e validar valores internacionalizados</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-json.git">laminas/laminas-json</a>
    </td>
    <td>biblioteca</td>
    <td>fornece métodos convenientes para serializar PHP nativo para JSON e decodificar JSON para PHP nativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-loader.git">laminas/laminas-loader</a>
    </td>
    <td>biblioteca</td>
    <td>Estratégias de carregamento automático e de plug-in</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-modulemanager.git">laminas/laminas-modulemanager</a>
    </td>
    <td>biblioteca</td>
    <td>Sistema de aplicação modular para aplicações em lamininas-mvc</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mvc.git">laminas/laminas-mvc</a>
    </td>
    <td>biblioteca</td>
    <td>Camada MVC orientada por eventos do Laminas, incluindo aplicativos MVC, controladores e plug-ins</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-permissions-acl.git">laminas/laminas-permissions-acl</a>
    </td>
    <td>biblioteca</td>
    <td>Oferece uma implementação leve e flexível de ACL (Access Control List, lista de controle de acesso) para gerenciamento de privilégios</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-recaptcha.git">laminas/laminas-recaptcha</a>
    </td>
    <td>biblioteca</td>
    <td>Invólucro OOP para o serviço Web ReCaptcha</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-router.git">laminas/laminas-router</a>
    </td>
    <td>biblioteca</td>
    <td>Sistema de roteamento flexível para aplicativos HTTP e de console</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-server.git">laminas/laminas-server</a>
    </td>
    <td>biblioteca</td>
    <td>Criar servidores RPC baseados em Reflexo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-servicemanager.git">laminas/laminas-servicemanager</a>
    </td>
    <td>biblioteca</td>
    <td>Contêiner de injeção de dependência de fábrica</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-session.git">laminas/laminas-session</a>
    </td>
    <td>biblioteca</td>
    <td>Interface orientada a objetos para sessões e armazenamento em PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-soap.git">laminas/laminas-soap</a>
    </td>
    <td>biblioteca</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-stdlib.git">laminas/laminas-stdlib</a>
    </td>
    <td>biblioteca</td>
    <td>Extensões SPL, utilitários de matriz, manipuladores de erros e muito mais</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-text.git">laminas/laminas-text</a>
    </td>
    <td>biblioteca</td>
    <td>Criar FIGlets e tabelas baseadas em texto</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-translator.git">laminas/laminas-translator</a>
    </td>
    <td>biblioteca</td>
    <td>Interfaces para o componente tradutor de laminas-i18n</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-uri.git">laminas/laminas-uri</a>
    </td>
    <td>biblioteca</td>
    <td>Um componente que auxilia na manipulação e validação dos URIs (Uniform Resource Identifiers, identificadores uniformes de recursos)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-validator.git">laminas/laminas-validator</a>
    </td>
    <td>biblioteca</td>
    <td>Classes de validação para uma grande variedade de domínios e a capacidade de encadear validadores para criar critérios de validação complexos</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-view.git">laminas/laminas-view</a>
    </td>
    <td>biblioteca</td>
    <td>Camada de visualização flexível que suporta e fornece várias camadas de visualização, auxiliares e muito mais</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/marc-mabe/php-enum.git">marc-mabe/php-enum</a>
    </td>
    <td>biblioteca</td>
    <td>Implementação simples e rápida de enumerações com PHP nativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/nikic/PHP-Parser.git">nikic/php-parser</a>
    </td>
    <td>biblioteca</td>
    <td>Um analisador PHP escrito em PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpfui/recaptcha.git">phpfui/recaptcha</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca cliente do reCAPTCHA do Google para PHP 8.4 e superior</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tedious/JShrink.git">tedivm/jshrink</a>
    </td>
    <td>biblioteca</td>
    <td>Minificador JavaScript construído no PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port.git">tubalmartin/cssmin</a>
    </td>
    <td>biblioteca</td>
    <td>Uma porta PHP do compactador CSS YUI</td>
  </tr>
  </tbody>
</table>

### BSD-3-Cláusula-Modificação

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_Redis.git">colinmollenhour/cache-backend-redis</a>
    </td>
    <td>magento-module</td>
    <td>Infraestrutura Zend_Cache usando Redis com suporte completo para tags.</td>
  </tr>
  </tbody>
</table>

### ISC

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/paragonie/sodium_compat.git">paragonie/dium_compat</a>
    </td>
    <td>biblioteca</td>
    <td>Implementação PHP pura de libsódio; usa a extensão PHP se ela existir</td>
  </tr>
  </tbody>
</table>

### LGPL-2.1 ou posterior

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/ezyang/htmlpurifier.git">ezyang/htmlpurifier</a>
    </td>
    <td>biblioteca</td>
    <td>Filtro HTML compatível com padrões escrito em PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-amqplib/php-amqplib.git">php-amqplib/php-amqplib</a>
    </td>
    <td>biblioteca</td>
    <td>Anteriormente videlalvaro/php-amqplib.  Esta biblioteca é uma implementação PHP pura do protocolo AMQP. Foi testado contra RabbitMQ.</td>
  </tr>
  </tbody>
</table>

### MIT

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/braintree/braintree_php.git">braintree/braintree_php</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca cliente PHP Braintree</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/math.git">brick/math</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca aritmética de precisão arbitrária</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/varexporter.git">exportador/brick</a>
    </td>
    <td>biblioteca</td>
    <td>Uma alternativa poderosa para var_export(), que pode exportar fechamentos e objetos sem __set_state()</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon-doctrine-types.git">carbonphp/carbon-doutr-types</a>
    </td>
    <td>biblioteca</td>
    <td>Tipos de Carbono na Doutrina</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ChristianRiesen/base32.git">christian-riesen/base32</a>
    </td>
    <td>biblioteca</td>
    <td>Codificador/decodificador Base32 de acordo com RFC 4648</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/credis.git">colinmollenhour/credis</a>
    </td>
    <td>biblioteca</td>
    <td>Credis é uma interface leve para a loja de valores-chave Redis que envolve a biblioteca phpredis quando disponível para melhor desempenho.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/ca-bundle.git">composer/ca-bundle</a>
    </td>
    <td>biblioteca</td>
    <td>Permite encontrar um caminho para o pacote de CA do sistema e inclui um fallback para o pacote de CA Mozilla.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/class-map-generator.git">composer/class-map-generator</a>
    </td>
    <td>biblioteca</td>
    <td>Utilitários para escanear código PHP e gerar mapas de classe.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/composer.git">compositor/compositor</a>
    </td>
    <td>biblioteca</td>
    <td>O Composer ajuda você a declarar, gerenciar e instalar dependências de projetos PHP. Ele garante que você tenha a pilha certa em todos os lugares.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/metadata-minifier.git">compositor/minificador de metadados</a>
    </td>
    <td>biblioteca</td>
    <td>Pequena biblioteca de utilitários que lida com minificação e expansão de metadados.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/pcre.git">compositor/pcre</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de encapsulamento PCRE que oferece substituições preg_* seguras para tipos.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/semver.git">compositor/semver</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de servidor que oferece utilitários, análise de restrição de versão e validação.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/spdx-licenses.git">composer/spdx-licenses</a>
    </td>
    <td>biblioteca</td>
    <td>Lista de licenças SPDX e biblioteca de validação.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/xdebug-handler.git">composer/xdebug-handler</a>
    </td>
    <td>biblioteca</td>
    <td>Reinicia um processo sem o Xdebug.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/doctrine/lexer.git">doutrina/lexer</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de analisadores Lexer de Doutrina PHP que pode ser usada em Analisadores Descendentes Recursivos.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/egulias/EmailValidator.git">guias/validador de email</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca para validar emails em relação a vários RFCs</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/elastic/elastic-transport-php.git">elástico/transporte</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca PHP de transporte HTTP para produtos Elastic</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/elastic/elasticsearch-php.git">elasticsearch/elasticsearch</a>
    </td>
    <td>biblioteca</td>
    <td>Cliente PHP para Elasticsearch</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/endroid/qr-code.git">endroid/qr-code</a>
    </td>
    <td>biblioteca</td>
    <td>Código QR Endroid</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/guzzlestreams.git">ezimuel/guzzlestreams</a>
    </td>
    <td>biblioteca</td>
    <td>Bifurcação de garoa/transmissões (abandonada) para ser usada com elasticsearch-php</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/ringphp.git">ezimuel/ringphp</a>
    </td>
    <td>biblioteca</td>
    <td>Bifurcação de guzzle/RingPHP (abandonada) para ser usada com elasticsearch-php</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/FriendsOfPHP/proxy-manager-lts.git">friendsofphp/proxy-manager-lts</a>
    </td>
    <td>biblioteca</td>
    <td>Adição de suporte para uma gama mais ampla de versões do PHP ao ocramius/proxy-manager</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/bzikarsky/gelf-php.git">graylog2/gelf-php</a>
    </td>
    <td>biblioteca</td>
    <td>Uma implementação php para enviar mensagens de registro para um backend compatível com o GELF como o Graylog2.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/guzzle.git">guzzlehttp/guzzle</a>
    </td>
    <td>biblioteca</td>
    <td>Guzzle é uma biblioteca cliente HTTP PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/promises.git">guzzlehttp/promessas</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de promessas do Guzzle</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/psr7.git">guzzlehttp/psr7</a>
    </td>
    <td>biblioteca</td>
    <td>Implementação de mensagem PSR-7 que também fornece métodos de utilitário comuns</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/collections.git">iluminar/coleções</a>
    </td>
    <td>biblioteca</td>
    <td>O pacote Illuminate Collections.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/conditionable.git">iluminar/condicionar</a>
    </td>
    <td>biblioteca</td>
    <td>O pacote Iluminar condicional.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/config.git">iluminar/configurar</a>
    </td>
    <td>biblioteca</td>
    <td>O pacote Illuminate Config.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/contracts.git">iluminar/contratos</a>
    </td>
    <td>biblioteca</td>
    <td>O pacote Iluminar Contratos.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/macroable.git">iluminar/macrosável</a>
    </td>
    <td>biblioteca</td>
    <td>O pacote Iluminar macroable.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jsonrainbow/json-schema.git">justinarco-íris/json-schema</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca para validar um esquema json.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem.git">liga/flysystem</a>
    </td>
    <td>biblioteca</td>
    <td>Abstração de armazenamento de arquivo para PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-aws-s3-v3.git">liga/flysystem-aws-s3-v3</a>
    </td>
    <td>biblioteca</td>
    <td>Adaptador de sistema de arquivos AWS S3 para Flysystem.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-local.git">liga/flysystem-local</a>
    </td>
    <td>biblioteca</td>
    <td>Adaptador de sistema de arquivos local para Flysystem.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/mime-type-detection.git">league/mime-type-detection</a>
    </td>
    <td>biblioteca</td>
    <td>Detecção de tipo MIME para Flysystem</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/monolog.git">monólogo/monólogo</a>
    </td>
    <td>biblioteca</td>
    <td>Envia seus registros para arquivos, soquetes, caixas de entrada, bancos de dados e vários serviços da Web</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jmespath/jmespath.php.git">mtdowling/jmespath.php</a>
    </td>
    <td>biblioteca</td>
    <td>Especificar declarativamente como extrair elementos de um documento JSON</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon.git">nesbot/carbono</a>
    </td>
    <td>biblioteca</td>
    <td>Uma extensão de API para DateTime que suporta 281 idiomas diferentes.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/constant_time_encoding.git">paragonie/constant_time_encoding</a>
    </td>
    <td>biblioteca</td>
    <td>Implementações de tempo constante da codificação RFC 4648 (Base-64, Base-32, Base-16)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/random_compat.git">paragonie/random_compat</a>
    </td>
    <td>biblioteca</td>
    <td>PHP 5.x polyfill para random_bytes() e random_int() do PHP 7</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/emogrifier.git">pelago/emotificador</a>
    </td>
    <td>biblioteca</td>
    <td>Converte estilos CSS em atributos de estilo em linha no seu código HTML</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/discovery.git">php-http/discovery</a>
    </td>
    <td>composer-plugin</td>
    <td>Localiza e instala implementações PSR-7, PSR-17, PSR-18 e HTTPlug</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/httplug.git">php-http/httplug</a>
    </td>
    <td>biblioteca</td>
    <td>HTTPlug, a abstração do cliente HTTP para PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-http/promise.git">php-http/promessa</a>
    </td>
    <td>biblioteca</td>
    <td>Promessa usada para solicitações HTTP assíncronas</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/CssXPath.git">phpgt/cssxpath</a>
    </td>
    <td>biblioteca</td>
    <td>Converta seletores de CSS em consultas XPath.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpgt/Dom.git">phpgt/dom</a>
    </td>
    <td>biblioteca</td>
    <td>API DOM moderna.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/PropFunc.git">phpgt/propfunc</a>
    </td>
    <td>biblioteca</td>
    <td>Funções de acessador e mutador de propriedade.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/mcrypt_compat.git">phpseclib/mcrypt_compat</a>
    </td>
    <td>biblioteca</td>
    <td>polyfill do PHP 5.x-8.x para extensão mcrypt</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/phpseclib.git">phpseclib/phpseclib</a>
    </td>
    <td>biblioteca</td>
    <td>Biblioteca de comunicações seguras PHP - implementações de PHP puro de RSA, AES, SSH2, SFTP, X.509 etc.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/cache.git">psr/cache</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum para o armazenamento em cache de bibliotecas</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/clock.git">psr/clock</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum para a leitura do relógio.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/container.git">psr/container</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum de contêiner (PHP FIG PSR-11)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/event-dispatcher.git">psr/event-dispatcher</a>
    </td>
    <td>biblioteca</td>
    <td>Interfaces padrão para manipulação de eventos.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-client.git">psr/http-client</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum para clientes HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-factory.git">psr/http-fatory</a>
    </td>
    <td>biblioteca</td>
    <td>PSR-17: interfaces comuns para fábricas de mensagens HTTP PSR-7</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-message.git">psr/http-message</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum para mensagens HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/log.git">psr/log</a>
    </td>
    <td>biblioteca</td>
    <td>Interface comum para bibliotecas de registro</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/simple-cache.git">psr/simple-cache</a>
    </td>
    <td>biblioteca</td>
    <td>Interfaces comuns para cache simples</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ralouphie/getallheaders.git">ralouphie/getallheaders</a>
    </td>
    <td>biblioteca</td>
    <td>Um polyfill para getallheaders.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/collection.git">ramsey/collection</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca PHP para representar e manipular coleções.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/uuid.git">ramsey/uuid</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca PHP para geração e trabalho com identificadores universalmente exclusivos (UUIDs).</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/reactphp/promise.git">reação/promessa</a>
    </td>
    <td>biblioteca</td>
    <td>Uma implementação leve do CommonJS Promises/A para PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/PHP-CSS-Parser.git">sabberworm/php-css-parser</a>
    </td>
    <td>biblioteca</td>
    <td>Analisador para arquivos CSS escritos em PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/jsonlint.git">seld/jsonlint</a>
    </td>
    <td>biblioteca</td>
    <td>Linter JSON</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/phar-utils.git">seld/phar-utils</a>
    </td>
    <td>biblioteca</td>
    <td>utilitários de formato de arquivo PHAR, para quando o PHP lhe mostrar</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/signal-handler.git">seld/manipulador de sinal</a>
    </td>
    <td>biblioteca</td>
    <td>Manipulador simples de sinais Unix que falha silenciosamente quando os sinais não são suportados para um fácil desenvolvimento entre plataformas</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/aes-key-wrap.git">spomky-labs/aes-key-wrap</a>
    </td>
    <td>biblioteca</td>
    <td>Encapsulamento de chave AES para PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/otphp.git">spomky-labs/otphp</a>
    </td>
    <td>biblioteca</td>
    <td>Uma biblioteca PHP para gerar senhas únicas de acordo com RFC 4226 (algoritmo HOTP) e RFC 6238 (algoritmo TOTP) e compatível com o Google Authenticator</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/pki-framework.git">spomky-labs/pki-framework</a>
    </td>
    <td>biblioteca</td>
    <td>Uma estrutura PHP para gerenciamento da Infraestrutura de Chave Pública. Ele inclui certificados de chave pública X.509, certificados de atributo, solicitações de certificação e validação do caminho de certificação.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/clock.git">symfony/clock</a>
    </td>
    <td>biblioteca</td>
    <td>Dissocia os aplicativos do relógio do sistema</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/config.git">symfony/config</a>
    </td>
    <td>biblioteca</td>
    <td>Ajuda a localizar, carregar, combinar, preencher automaticamente e validar valores de configuração de qualquer tipo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/console.git">symfony/console</a>
    </td>
    <td>biblioteca</td>
    <td>Facilita a criação de interfaces de linha de comando belas e testáveis</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/css-selector.git">symfony/css-seletor</a>
    </td>
    <td>biblioteca</td>
    <td>Converte seletores CSS em expressões XPath</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/dependency-injection.git">injeção de symfony/dependência</a>
    </td>
    <td>biblioteca</td>
    <td>Permite padronizar e centralizar a maneira como os objetos são construídos em seu aplicativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/deprecation-contracts.git">symfony/deprecation-contracts</a>
    </td>
    <td>biblioteca</td>
    <td>Uma função e convenção genéricas para acionar avisos de substituição</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/error-handler.git">symfony/error-handler</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece ferramentas para gerenciar erros e facilitar a depuração do código PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher.git">symfony/event-dispatcher</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece ferramentas que permitem que os componentes do aplicativo se comuniquem entre si despachando eventos e ouvindo-os</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher-contracts.git">symfony/event-dispatcher-contracts</a>
    </td>
    <td>biblioteca</td>
    <td>Abstrações genéricas relacionadas ao evento de despacho</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/filesystem.git">symfony/filesystem</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece utilitários básicos para o sistema de arquivos</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/finder.git">symfony/finder</a>
    </td>
    <td>biblioteca</td>
    <td>Localiza arquivos e diretórios por meio de uma interface intuitiva e fluente</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client.git">symfony/http-client</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece métodos avançados para buscar recursos HTTP de forma síncrona ou assíncrona</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client-contracts.git">symfony/http-client-contracts</a>
    </td>
    <td>biblioteca</td>
    <td>Abstrações genéricas relacionadas a clientes HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-foundation.git">symfony/http-foundation</a>
    </td>
    <td>biblioteca</td>
    <td>Define uma camada orientada a objetos para a especificação HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-kernel.git">symfony/http-kernel</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece um processo estruturado para converter uma Solicitação em uma Resposta</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/intl.git">symfony/intl</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece acesso aos dados de localização da biblioteca ICU</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/mailer.git">symfony/mailer</a>
    </td>
    <td>biblioteca</td>
    <td>Ajuda a enviar emails</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/mime.git">symfony/mime</a>
    </td>
    <td>biblioteca</td>
    <td>Permite manipular mensagens MIME</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-ctype.git">symfony/polyfill-ctype</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill para funções ctype</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-grapheme.git">symfony/polyfill-intl-grapheme</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill para funções grapheme_* da intl</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-idn.git">symfony/polyfill-intl-idn</a>
    </td>
    <td>biblioteca</td>
    <td>polyfill Symfony para as funções idn_to_ascii e idn_to_utf8 da intl</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-normalizer.git">symfony/polyfill-intl-normalizer</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill para a classe Normalizer da intl e funções relacionadas</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-mbstring.git">symfony/polyfill-mbstring</a>
    </td>
    <td>biblioteca</td>
    <td>Preenchimento múltiplo Symfony para a extensão Mbstring</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php73.git">symfony/polyfill-php73</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 7.3+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php80.git">symfony/polyfill-php80</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.0+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php81.git">symfony/polyfill-php81</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.1+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php82.git">symfony/polyfill-php82</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.2+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php83.git">symfony/polyfill-php83</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.3+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php84.git">symfony/polyfill-php84</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.4+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php85.git">symfony/polyfill-php85</a>
    </td>
    <td>biblioteca</td>
    <td>Symfony polyfill reforçando alguns recursos do PHP 8.5+ para versões mais baixas do PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/process.git">symfony/process</a>
    </td>
    <td>biblioteca</td>
    <td>Executa comandos em subprocessos</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/proxy-manager-bridge.git">symfony/proxy-manager-bridge</a>
    </td>
    <td>symfony-bridge</td>
    <td>Fornece integração do ProxyManager com vários componentes do Symfony</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/serializer.git">symfony/serializer</a>
    </td>
    <td>biblioteca</td>
    <td>Lida com a serialização e desserialização de estruturas de dados, incluindo gráficos de objetos, em estruturas de matriz ou outros formatos como XML e JSON.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/service-contracts.git">symfony/service-contracts</a>
    </td>
    <td>biblioteca</td>
    <td>Abstrações genéricas relacionadas com serviços de escrita</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/string.git">symfony/string</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece uma API orientada a objetos para strings e lida com bytes, pontos de código UTF-8 e clusters de grapheme de forma unificada</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation.git">symfony/translation</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece ferramentas para internacionalizar seu aplicativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation-contracts.git">contratos-symfony/translation</a>
    </td>
    <td>biblioteca</td>
    <td>Abstrações genéricas relacionadas à tradução</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-dumper.git">symfony/var-dumper</a>
    </td>
    <td>biblioteca</td>
    <td>Fornece mecanismos para percorrer qualquer variável PHP arbitrária</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-exporter.git">symfony/var-exporter</a>
    </td>
    <td>biblioteca</td>
    <td>Permite exportar qualquer estrutura de dados PHP serializável para código PHP simples</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/yaml.git">symfony/yaml</a>
    </td>
    <td>biblioteca</td>
    <td>Carrega e despeja arquivos YAML</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/web-token/jwt-framework.git">web-token/jwt-framework</a>
    </td>
    <td>symfony-bundle</td>
    <td>Biblioteca JSON Object Signing and Encryption para PHP e Symfony Bundle.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webonyx/graphql-php.git">webonyx/graphql-php</a>
    </td>
    <td>biblioteca</td>
    <td>Uma porta PHP da implementação de referência do GraphQL</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/zordius/lightncandy.git">zordius/lightncandy</a>
    </td>
    <td>biblioteca</td>
    <td>Uma implementação PHP extremamente rápida de handlebars ( http://handlebarsjs.com/ ) e bigode ( http://mustache.github.io/ ).</td>
  </tr>
  </tbody>
</table>

### OSL-3.0, AFL-3.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-customer-balance
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-vale-presente
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-vale-presente-conta
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-present-wrapping
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-graph-ql
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-award
    </td>
    <td>magento2-module</td>
    <td>N/D</td>
  </tr>
  </tbody>
</table>

### OSL-3.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### PHP

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/2tvenom/CBOREncode.git">2tvenom/cborencode</a>
    </td>
    <td>biblioteca</td>
    <td>Codificador CBOR para PHP</td>
  </tr>
  </tbody>
</table>

### Proprietário

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### proprietário

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrição</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-core
    </td>
    <td>magento2-module</td>
    <td>Bifurcação do módulo Magento Braintree 2.2.0 de Gene Commerce para PayPal.</td>
  </tr>
  </tbody>
</table>
