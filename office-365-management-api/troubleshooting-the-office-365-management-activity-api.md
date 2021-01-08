---
ms.technology: o365-service-communications
ms.TocTitle: Troubleshooting the Office 365 Management Activity API
title: Solução de problemas da API da Atividade de Gerenciamento do Office 365
description: Resume as perguntas mais comuns que o Suporte da Microsoft recebe no suporte a essa API.
ms.ContentId: 50822603-a1ec-a754-e7dc-67afe36bb1b0
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 9c909220d660e0202c3ebda2777b2d8922da45a3
ms.sourcegitcommit: c3bb30b86a4569e9f18891f1cdc30cbffc8c8db4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "49784204"
---
# <a name="office-365-management-activity-api-faqs-and-troubleshooting"></a>Perguntas frequentes e Soluções de problemas da API da Atividade de Gestão do Office 365

A API da Atividade de Gestão do Office 365 (também conhecida como a *API de Auditoria Unificada*) é apenas uma parte das ofertas de conformidade e segurança do Office 365, que:

- Permite o acesso programático a várias cargas de trabalho de pipeline de auditoria (como o SharePoint e o Exchange)

- É a principal interface usada por uma variedade de produtos de fornecedores de terceiros para agregar e indexar dados de auditoria

A API da Atividade de Gestão não deve ser confundida com a API de Comunicações de Serviço do Office 365. A API da Atividade de Gerenciamento serve para auditar as atividades do usuário final em várias cargas de trabalho. A API do Serviço de Comunicações serve para auditar o status e as mensagens enviadas pelos serviços disponíveis no Office 365 (como Dynamics CRM ou Serviço de Identidade).
 
> [!NOTE]
> Houve um problema com eventos pertencentes ao tipo de conteúdo Audit.AzureActiveDirectory não disponíveis por meio da API da Atividade de Gestão do Office 365 entre 22 de outubro de 2020 e 6 de novembro de 2020. Os eventos de entrada do Microsoft Azure Active Directory não foram afetados por esse problema. Os eventos ausentes para o período de impacto estarão disponíveis nos próximos dias, e espera-se que sejam concluídos o mais tardar até 20 de novembro de 2020. Em alguns casos, os clientes podem notar dados de eventos duplicados para eventos gerados entre 26 de outubro de 2020 e 5 de novembro de 2020.

## <a name="frequently-asked-questions-about-the-office-365-management-activity-api"></a>Perguntas frequentes sobre a API da Atividade de Gestão do Office 365

**Como faço para integrar a API de Atividade de Gerenciamento?**

Para começar a usar a API da Atividade de Gestão do Office 365, confira [ Introdução às APIs de Gerenciamento do Office 365 ](get-started-with-office-365-management-apis.md).

**O que acontece se eu desabilitar a auditoria para a minha organização do Office 365? Ainda terei os eventos pela API da Atividade de Gestão?**

Não. A auditoria unificada do Office 365 deve estar habilitada na sua organização para incluir os registros por meio da API de Atividade de Gestão. Para obter instruções, confira [Ativar ou desativar a pesquisa de log de auditoria](https://docs.microsoft.com/microsoft-365/compliance/turn-audit-log-search-on-or-off).

**Que eventos são auditados para um serviço específico do Office 365?**

A documentação do esquema de API da Atividade de Gestão do Office 365 possui uma lista abrangente de eventos. Para ver mais detalhes, confira o esquema de API da Atividade de Gestão do Office 365. Confira também a seção "atividades auditadas" em [Pesquisar o log de auditoria no Centro de Conformidade e Segurança](https://docs.microsoft.com/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#audited-activities) para obter uma lista de eventos para a maioria dos serviços do Office 365 que são auditados.

**Há alguma diferença entre os registros buscados pela API da Atividade de Gestão e os registros retornados usando a ferramenta de pesquisa de log de auditoria no Centro de Conformidade do Microsoft 365?**

Os dados retornados pelos dois métodos são os mesmos. A única diferença é que, com a API, você pode obter dados apenas dos últimos sete dias (mais detalhes nas perguntas a seguir). Ao pesquisar o log de auditoria no Centro de Conformidade e Segurança (ou usando o cmdlet **Search-UnifiedAuditLog** correspondente no Exchange Online PowerShell), é possível obter dados do período de retenção ativos quando o dado foi gerado (por exemplo, 90 dias ou uma ano).

**Qual é o tempo máximo que terei que esperar antes de uma notificação ser enviada sobre um determinado evento do Office 365?**

Não há nenhuma latência máxima garantida para a entrega de notificação (ou seja, nenhum SLA). Geralmente, a maioria das notificações é enviada dentro de uma hora do evento. Geralmente, a latência é muito mais curta, mas esse período pode ser mais demorado, de acordo com a carga de trabalho.

**As notificações webhook não são mais imediatas?**

Não. Recentemente, o tempo de espera de entrega de notificações ao usar um webhook tem sido maior se comparado à consulta da API diretamente com a operação `/content`.

**Quanto tempo o conteúdo permanecerá disponível para buscar pela API?**

O conteúdo está disponível para ser obtido pela API por 7 dias após a notificação da disponibilidade do conteúdo. Mesmo que a notificação atrase por um período excepcionalmente longo (por exemplo, no caso de uma interrupção do serviço), você ainda terá sete dias após a primeira disponibilidade da notificação para baixar o blob de conteúdo relacionado ao evento de origem.

**Posso consultar a API da Atividade de Gestão para uma ID de evento específico ou RecordType ou outras propriedades no blob de conteúdo?**

Não. A API de Gerenciamento de Atividade fornece todos os detalhes do evento para um log específico. Pode ser usado para baixar todos os detalhes e, em seguida, você pode implementar sua própria lógica de consulta nos dados baixados; por exemplo, usando um aplicativo personalizado do ou uma ferramenta de terceiros.

**Como posso saber se estão precisos e completos os dados provenientes da minha solução de auditoria existente que coleta dados da API da Atividade de Gerenciamento?**

A resposta curta é que a Microsoft não fornece nenhum tipo de log que permitirá verificar aplicativos ou aplicativos de terceiros (ISV). Há outros produtos de segurança da Microsoft que usam os dados da mesmo pipeline, mas esses produtos estão fora do escopo dessa discussão e não podem ser usados diretamente para verificar a API da Atividade de Gestão. Se você estiver preocupado com discrepâncias entre o que sua solução existente fornece e o que espera, implemente as operações ilustradas acima. Mas isso pode ser difícil, dependendo de como sua ferramenta ou solução existente lista e indexa os dados. Se sua solução existente apenas apresentar dados classificados por hora de criação do evento real, não será possível consultar a API por hora de criação do evento, para que você possa comparar conjuntos de resultados. Nesse cenário, você precisará coletar os blobs de conteúdo notificados por vários dias, indexá-los ou classificá-los manualmente e fazer uma comparação simples.

**O que é a restrição de limitação para a API da Atividade de Gestão?**

Todas as organizações alocam inicialmente uma linha de base de 2.000 solicitações por minuto. Em seguida, o limite é ajustado com base em uma combinação de fatores, incluindo o número de assentos na organização. Além disso, as organizações do Office 365 E5 e do Microsoft 365 E5 terão aproximadamente duas vezes mais largura de banda quanto organizações não-e5. Também haverá limite máximo quanto à largura de banda para proteger a integridade do serviço.

> [!NOTE]
> Embora cada locatário possa enviar inicialmente até 2 mil solicitações por minuto, a Microsoft não pode garantir uma taxa de resposta. A taxa de resposta depende de vários fatores, como o desempenho do sistema do cliente, a capacidade e a velocidade da rede.

**Estou encontrando um erro de limitação na API da Atividade de Gestão. O que devo fazer?**

Abra um tíquete com o Suporte da Microsoft e solicite uma nova restrição de limitação, além de incluir uma justificativa comercial para aumentar o limite. Nós avaliaremos a solicitação e, se aceita, aumentaremos a restrição de limitação.

**Por que as TargetUpdatedProperties não estão mais em ExtendedProperties nos logs de auditoria das atividades do Azure Active Directory?**

As TargetUpdatedProperties aparecem em ExtendedProperties. No entanto, eles foram removidos das ExtendedProperties e aparecerão em ModifiedProperties.

**Por que os logs de auditoria para erros de UserAccountNotFound para atividades de entrada do Azure Active Directory (Azure AD) não estão disponíveis por meio da API de atividade de gerenciamento?**

A partir de novembro de 2020, os logs de auditoria para atividades de entrada do Azure AD serão inseridos no log de auditoria unificado dos Hubs de Eventos do Microsoft Azure Active Directory. Como os erros de logon UserAccountNotFound não estão disponíveis nos Hubs de Eventos, os logs de auditoria para erros UserAccountNotFound não são mais retornados pela API de Atividade de Gerenciamento.

## <a name="troubleshooting-the-office-365-management-activity-api"></a>Solução de problemas da API da Atividade de Gestão do Office 365

Qualquer pessoa que comece a usar a API da Atividade de Gestão do Office 365 deve entender que não há um conceito de consulta por informações específicas do evento, como a data em que ocorreu o evento, de qual coleção de sites um evento pode ter sido acionado ou o tipo de evento. Em vez disso, você cria assinaturas para cargas de trabalho específicas (por exemplo, Microsoft Office SharePoint Online ou Microsoft Azure Active Directory) e cada assinatura é por locatário.

As seções a seguir resumem as perguntas mais comuns que os clientes fazem ao usar a API da Atividade de Gestão do Office 365:

- [Perguntas sobre clientes e ferramentas de terceiros](#questions-about-third-party-tools-and-clients)

- [Habilitar o log de auditoria unificada no Office 365](#enabling-unified-audit-logging-in-office-365)

- [Como conectar-se com a API](#connecting-to-the-api)

- [Como verificar suas assinaturas](#checking-your-subscriptions)

- [Como criar uma nova assinatura](#creating-a-new-subscription)

- [Como usar webhooks](#using-webhooks)

- [Como solicitar blobs de conteúdo e limitação](#requesting-content-blobs-and-throttling)

Mostraremos uma seleção de scripts simples do Windows PowerShell que podem ajudá-lo a responder às perguntas mais comuns feitas por clientes ou ajudar você a começar a implementar uma solução personalizada demonstrando as operações principais. Nem todas as operações são explicadas neste artigo, mas estão listadas na referência da API da Atividade de Gestão do Office 365.

### <a name="questions-about-third-party-tools-and-clients"></a>Perguntas sobre clientes e ferramentas de terceiros

A categoria mais comum de perguntas que recebemos de clientes que usam produtos de terceiros para baixar e agregar dados de auditoria. Dependendo do produto de terceiros, os clientes podem encontrar dificuldades com a configuração ou observar uma interrupção ou inconsistência nos dados expostos nesses produtos. Devemos mencionar que a primeira ação que esses clientes devem realizar é entrar em contato com o departamento de suporte do fornecedor. Geralmente, uma configuração de fornecedor específica do locatário ou um problema de serviço é a causa das queixas do cliente.

### <a name="enabling-unified-audit-logging-in-office-365"></a>Habilitar o log de auditoria unificada no Office 365

Se você acabou de configurar um aplicativo que está tentando usar a API de Atividade de Gerenciamento e não estiver funcionando, certifique-se de que ativou o registro em log de auditoria unificada para a sua organização do Office 365. Para fazer isso, ative o log de auditoria do Office 365. Para obter instruções, confira [Ativar ou desativar a pesquisa de log de auditoria do Office 365](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).

Se a auditoria unificada não estiver habilitada, uma mensagem de erro aparecerá contendo a cadeia de caracteres a seguir: `Microsoft.Office.Compliance.Audit``.DataServiceException: Tenant <tenantID> does not exist.`

### <a name="connecting-to-the-api"></a>Como conectar-se com a API

A maioria dos aplicativos conecta-se à API usando um fluxo OAuth2 simples de Credenciais de Cliente. Portanto, a primeira etapa é criar um aplicativo do Azure AD que tenha as permissões necessárias para acessar os dados da API da Atividade de Gerenciamento. Está fora do escopo deste artigo explicar as etapas para criar um registro de aplicativo do Microsoft Azure Active Directory. Para saber mais, confira [Registrar seu aplicativo com o locatário do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

#### <a name="azure-application-permissions"></a>Permissões de aplicativo do Azure

As três permissões usadas no momento para a API da Atividade de Gerenciamento do Office 365 são:

1. Ler dados de atividade para sua organização

2. Ler as informações de integridade do serviço para sua organização

3. Ler os eventos de política de Prevenção de Perda de Dados (DLP), incluindo informações confidenciais detectadas

> [!NOTE]
> Você deve habilitar as permissões de aplicativo e as permissões delegadas pelo menos para os primeiros dois dos conjuntos de permissões acima. Ler eventos de política DLP só será necessário se você estiver interessado nas cargas de trabalho DLP.

#### <a name="getting-an-access-token"></a>Como obter um token de acesso

O seguinte script do PowerShell usa a ID do aplicativo e o segredo do cliente para obter o token do OAuth2 do ponto de extremidade de autenticação da API da Atividade de Gerenciamento. Ele, em seguida, coloca o token de acesso na variável de matriz do `$headerParams`, que você anexará à sua solicitação HTTP. Para o valor do ponto de extremidade da API (na variável $resource), use um dos seguintes valores com base no plano de assinatura do Microsoft 365 ou do Office 365 da sua organização:

- Plano empresarial: `manage.office.com`

- Plano governamental do GCC: `manage-gcc.office.com`

- Plano de alto governo GCC: `manage.office365.us`

- Plano governamental DoD: `manage.protection.apps.mil`

```powershell
# Create app of type Web app / API in Azure AD, generate a Client Secret, and update the client id and client secret here
$ClientID = "<YOUR_APPLICATION_ID"
$ClientSecret = "<YOUR_CLIENT_SECRET>"
$loginURL = "https://login.microsoftonline.com/"
$tenantdomain = "<YOUR_DOMAIN>.onmicrosoft.com"
# Get the tenant GUID from Properties | Directory ID under the Azure Active Directory section. For $resource, use one of these endpoint values based on your subscription plan: Enterprise - manage.office.com; GCC - manage-gcc.office.com; GCC High: manage.office365.us; DoD: manage.protection.apps.mil
$TenantGUID = "<YOUR_TENANT_GUID>"
$resource = "https://<YOUR_API_ENDPOINT>"
# auth
$body = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 
```

A variável `$oauth` conterá o objeto de resposta que tem várias propriedades, incluindo o token de acesso. Um exemplo de resposta:

```json
token_type     : Bearer
expires_in     : 3599
ext_expires_in : 0
expires_on     : 1508285860
not_before     : 1508281960
resource       : https://manage.office.com
access_token   : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyIsImtpZCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyJ9.eyJhdWQiOiJodHRwczov…
```

### <a name="checking-your-subscriptions"></a>Como verificar suas assinaturas

Se você já passou por uma interrupção no fluxo de dados para uma solução ou cliente de API da Atividade de Gestão existente, pode estar se perguntando se algo aconteceu com a sua assinatura. Para verificar suas assinaturas ativas, adicione o seguinte ao script anterior:

```powershell
Invoke-WebRequest -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" 
```

#### <a name="sample-response"></a>Resposta de amostra

```json
StatusCode        : 200
Status Description : OK
Content           : [{"contentType":"Audit.Exchange","status":"enabled","webhook":null},{"contentType":"Audit.SharePoint","status":"enabled","webhook":{"authId":"","address":"https://mvcwebapiwebhookreceiver.azurewebsite...
RawContent        : HTTP/1.1 200 OK
                    Pragma: no-cache
                    Content-Length: 266
                    Cache-Control: no-cache
                    Content-Type: application/json; charset=utf-8
                    Expires: -1
                    Server: Microsoft-IIS/8.5,Microsoft-IIS/8.5
                    X-AspNet-Versi...
Forms             : {}
Headers           : {[Pragma, no-cache], [Content-Length, 266], [Cache-Control, no-cache], [Content-Type, application/json; charset=utf-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 266
```

Isso informa que o locatário tem as assinaturas Audit.Exchange e Audit.SharePoint habilitadas. A assinatura do Exchange não tem webhook habilitado (nulo) e a assinatura do SharePoint tem um webhook habilitado com o endereço do ponto de extremidade registrado mostrado.

### <a name="creating-a-new-subscription"></a>Como criar uma nova assinatura

Para criar uma nova assinatura, use a operação /start. Para o ponto de extremidade da API, use um destes valores de base do seu plano de assinatura:

- Plano empresarial: `manage.office.com`

- Plano governamental do GCC: `manage-gcc.office.com`

- Plano de alto governo GCC: `manage.office365.us`

- Plano governamental DoD: `manage.protection.apps.mil`

```powershell
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "https://<YOUR_API_ENDPOINT>/api/v1.0/$tenantGUID/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"

> [!NOTE]
> Remember that `$headerParams` was populated in the first part of the script listed in the [Connecting to the API](#connecting-to-the-api) section in this article.

The previous code will create a new subscription to the Audit.AzureActiveDirectory content type, with a webhook that is null. You can then check your subscriptions using the code in the [Checking your subscriptions](#checking-your-subscriptions) section in this article.

## Checking content availability

To check what content blobs were created during a certain period, you can add the following line to the script in the “Connecting to the API” section.

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint"
```

O exemplo anterior receberá todas as notificações de conteúdo que foram disponibilizadas hoje, ou seja de 12:00 UTC até a hora atual. Se você quiser especificar um período diferente (tendo em mente que o período máximo para o qual você pode consultar é 24 horas), adicione os parâmetros *starttime* e *endtime* ao URI; por exemplo:

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint&startTime=2017-10-13T00:00&endTime=2017-10-13T11:59"
```

> [!NOTE]
> Você deve usar os parâmetros *starttime* e *endtime* ou nenhum.

```json
[{      "contentUri" : "https://<your_API_endpoint>/api/v1.0/<your_tenant_guid>/activity/feed/audit/20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentId" : "20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentType" : "Audit.SharePoint",
        "contentCreated" : "2017-10-13T18:00:51.748Z",
        "contentExpiration" : "2017-10-20T18:00:51.748Z",
}]
```

> [!IMPORTANT]
>
> - A propriedade *contentUri* é o URI onde você pode recuperar blobs de conteúdo. O blob em si é o que contém os detalhes do evento; ele inclui detalhes sobre eventos 1 – N. Embora possa haver 30 objetos JSON na coleção, pode haver vários eventos mais detalhados nesses 30 URIs de conteúdo.
>
> - A propriedade *contentCreated* não é a data de criação do evento que está sendo notificado. Esta é a data de criação da notificação. Os eventos detalhados nesse blob podem ter sido criados bem antes de o blob de conteúdo ter sido criado. Portanto, você nunca pode consultar a API diretamente para os eventos ocorridos durante um período determinado.

#### <a name="paging-contents-for-busy-tenants"></a>Como paginar conteúdo para locatários ocupados

Vários locatários maiores do Office 365 têm milhares de eventos que estão sendo gerados a cada hora. Se esse for o caso da sua organização e você tentar executar uma consulta por um período de 24 horas, como no exemplo acima, talvez seja necessário recuperar mais notificações que podem ser retornadas em uma resposta. Nesse caso, você precisará implementar um loop lógico de algum tipo sempre verificando os cabeçalhos de resposta para o valor do cabeçalho **NextPageUrl:**. Confira a seção Paginação na referência da API da Atividade de Gestão do Office 365 para obter mais detalhes.

```powershell
IF the NextPageUrl header is present in the response... 

THEN issue a new request substituting the Uri parameter in the above Invoke-WebRequest example for the value of the NextPageUrl header...
 
ELSE exit the loop
```

É difícil testar isso código de loop a menos que você tenha um locatário muito ativo. No nosso teste, tentamos executar milhares de operações de atualização em um script e não foi possível gerar um número grande o suficiente de notificações para exigir que o cabeçalho **NextPageUrl** fosse enviado.

### <a name="using-webhooks"></a>Como usar webhooks

Há duas maneiras de obter uma notificação informando que os blobs de conteúdo foram criados. A abordagem *push* é implementada com um ponto de extremidade de webhook, que é um aplicativo Web que você cria e hospeda por conta própria ou em uma plataforma de nuvem. Registre o webhook sempre que criar uma assinatura para um tipo de conteúdo auditado. Você também pode adicionar um registro de webhook a uma assinatura existente usando a abordagem mostrada abaixo. A abordagem *pull* exige que você consulte um determinado período de tempo (não mais de 24 horas) usando a operação /content. A resposta indicará quais blobs de conteúdo foram criados durante o período especificado.

Antes de adicionar um webhook, esteja ciente dos dois problemas a seguir:

1. Os webhooks estão perdendo a ênfase da Microsoft devido à dificuldade de depurar e solucionar problemas. Normalmente, você hospedaria um ponto de extremidade de WebApi que respondesse a solicitações de entrada. Muitos clientes usam ambientes de hospedagem (sob os quais não têm controle total) ou em ambientes locais (que têm dificuldades para permitir solicitações HTTP de entrada). O suporte observou muitos problemas em que as solicitações de entrada do pipeline de auditoria do Office 365 foram bloqueadas por um firewall ou roteador. Nesse caso, a API implementará um algoritmo de retirada, que pode ser confuso e resultar na desabilitação do webhook na assinatura.

2. O webhook deverá estar pronto para responder imediatamente a uma solicitação de validação depois que a operação de início for executada. Se houver um bug no aplicativo de webhook, a validação falhará e o webhook não será habilitado. Para saber mais sobre o esquema de solicitação de validação, confira a seção Validação de webhook na referência da API da Atividade de Gestão do Office 365. Você deve levar em consideração os desafios na produção de um webhook pronto para produção para responder a notificações da API da Atividade de Gerenciamento.

Se você decidir implementar um webhook, ele deverá ser testado para verificar se o ponto de extremidade está preparado para responder com uma resposta HTTP 200 à solicitação de validação e às solicitações de notificação normal antes de qualquer operação /start que especifica que um ponto de extremidade de webhook ser chamada pela primeira vez. Normalmente, você usaria uma solicitação fictícia da API para testar essa preparação. Leia com atenção e observe as seções Validação de webhook e Recuperando conteúdo na referência da API da Atividade de Gerenciamento do Office 365 para entender o esquema desses dois tipos de solicitações.

Para adicionar uma assinatura com um ponto de extremidade de webhook, adicione um parâmetro de corpo ao POST para o ponto de extremidade /start; por exemplo:

```json
$body = '{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }}'
$uri = "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed//subscriptions/start?contentType=Audit.SharePoint"
Invoke-RestMethod -Method Post -uri $uri -Headers $headerParams -Body $body
```

Imediatamente após essa chamada, uma solicitação de validação será enviada para o `https://webhook.myapp.com/o365/ …` e deve haver um ouvinte preparado para responder, de acordo com a descrição na seção Validação de webhook na referência da API da Atividade de Gerenciamento do Office 365. Seu ouvinte deve responder com HTTP 200. Se você executar a operação /list imediatamente nesse ponto, não verá o webhook mostrado como algo diferente de nulo até que a validação seja retornada.

#### <a name="checking-notifications-to-webhooks"></a>Como verificar notificações para webhooks

É importante distinguir entre a operação /notifications e a operação /content. Verificar as notificações só será relevante se você tiver assinado um ponto de extremidade de webhook. Esta operação informará se as tentativas de enviar notificações para seu webhook tiveram êxito ou não. Essa operação não deve ser usada para listar o conteúdo disponível. Para verificar a disponibilidade de conteúdo em relação ao que você pode já ter recebeu no seu webhook, use a operação /content. Mas primeiro verifique se há notificações de falhas usando a operação /notifications.

### <a name="requesting-content-blobs-and-throttling"></a>Como solicitar blobs de conteúdo e limitação

Depois que você tiver obtido uma lista de URIs de conteúdo, será necessário solicitar os blobs especificados pelas URIs. A seguir está um exemplo de solicitação de um blob de conteúdo (utilizando o terminal da API em manage.office.com para organizações empresariais) utilizando o Windows PowerShell. Esse exemplo supõe que você já usou o exemplo anterior na seção [Obtendo um token de acesso](#getting-an-access-token) deste artigo para obter um token de acesso e preencheu a variável `$headerParams` corretamente.

```powershell
# Get a content blob
$uri = 'https://manage.office.com/api/v1.0/<<your-tenant-guid>>/activity/feed/audit/<<ContentID>$audit_sharepoint$Audit_SharePoint'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

Observe o seguinte sobre a variável *$uri* no exemplo anterior:

- Usamos aspas simples para que os símbolos *$* não sejam interpretados como variáveis no Windows PowerShell

- O URI inteiro será retornado na propriedade *contentUri* da resposta à sua chamada anterior para o ponto de extremidade /content. Os tokens de espaço reservado mostrados são apenas para ilustração.

Ao tentar recuperar os blobs de conteúdo disponíveis, muitos clientes (principalmente os que trabalham com locatários ocupados) observaram erros parecidos com isso:

```json
Response Code 403: {'error':{'code':'AF429','message':'Too many requests. Method=GetBlob, PublisherId=00000000-0000-0000-0000-000000000000'}}
```

Isso provavelmente é devido à limitação. Observe que o valor do parâmetro PublisherId provavelmente indica que o cliente não especificou o *PublisherIdentifier* na solicitação. Além disso, tenha em mente que o nome do parâmetro correto é *PublisherIdentifier* mesmo que você veja *PublisherId* listado nas respostas de erro 403.

> [!NOTE]
> Na referência de API, o parâmetro *PublisherIdentifier* está listado em cada operação da API, mas ele também deverá será incluído na solicitação GET para a URL contentUri ao recuperar blobs de conteúdo.

Se você estiver fazendo chamadas simples de API para solucionar problemas (por exemplo, verificando se uma determinado assinatura está ativa), você pode omitir com segurança o parâmetro *PublisherIdentifier*, mas qualquer código que seja importante para uso de produção deverá incluir o parâmetro *PublisherIdentifier* em todas as chamadas.

Se você estiver implementando um cliente para o locatário da sua empresa, o *PublisherIdentifier* será o GUID de locatário. Se você estiver criando um aplicativo de ISV ou suplemento para vários clientes, o *PublisherIdentifier* será a GUID de locatário do ISV e não a GUID de locatário da empresa do usuário final.

Se você incluir o *PublisherIdentifier* válido, estará em um pool alocado com 60 mil solicitações por minuto por locatário. Este é um número de solicitações excepcionalmente grande. No entanto, se você não incluir o parâmetro *PublisherIdentifier*, você estará no pool geral alocado com 60 mil solicitações por minuto para todos os locatários. Nesse caso, você provavelmente achará que suas chamadas estão ficando limitadas. Para evitar isso, veja aqui como é possível solicitar um blob de conteúdo usando o *PublisherIdentifier*:

```json
$contentUri = ($response.Content | ConvertFrom-Json).contentUri[0]
$uri = $contentUri + '?PublisherIdentifier=82b24b6d-0591-4604-827b-705d55d0992f'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

O exemplo anterior pressupõe que a variável *$response* foi preenchida com a resposta a uma solicitação para o ponto de extremidade /content e que a variável *$headerParams* inclui um token de acesso válido. O script utiliza o primeiro item na matriz de URIs de conteúdo da resposta e invoca GET para fazer o download desse blog e colocá-lo na variável *$contents*. O código provavelmente percorrerá a coleção contentUri emitindo o GET para cada *contentUri*.
