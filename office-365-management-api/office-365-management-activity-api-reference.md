---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management Activity API reference
title: Referência da API da Atividade de Gerenciamento do Office 365
description: Use a API da Atividade de Gerenciamento do Office 365 para recuperar informações sobre ações e eventos de usuário, administração, sistema e políticas dos logs de atividades do Office 365 e do Azure AD.
ms.ContentId: 52749845-37f8-6076-7ea5-49d9a4055445
ms.topic: reference (API)
ms.date: ''
ms.localizationpriority: high
ms.openlocfilehash: b14820bb3970e1670e2828d73b18673bc5c51e54
ms.sourcegitcommit: c664ee51dfc7d4663e92883af3f0fec0e9af11c8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2022
ms.locfileid: "62202219"
---
# <a name="office-365-management-activity-api-reference"></a>Referência da API da Atividade de Gerenciamento do Office 365

Use a API da Atividade de Gerenciamento do Office 365 para recuperar informações sobre ações e eventos de usuário, administração, sistema e políticas dos logs de atividades do Office 365 e do Azure AD. 

Você pode usar as ações e os eventos dos logs de auditoria e atividades do Office 365 e do Microsoft Azure Active Directory para criar soluções que forneçam monitoramento, análise e visualização de dados. Essas soluções oferecem às organizações mais visibilidade das ações realizadas em relação a seu conteúdo. Essas ações e eventos também estão disponíveis nos Relatórios de Atividades do Office 365. Para obter mais informações, consulte [Pesquisar o log de auditoria no Microsoft 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance).

A API da Atividade de Gerenciamento do Office 365 é um serviço Web REST que você pode usar para desenvolver soluções usando qualquer linguagem e ambiente de hospedagem que dê suporte a certificados HTTPS e X.509. A API depende do Azure AD e do protocolo OAuth2 para autorização e autenticação. Para acessar a API por meio de seu aplicativo, primeiro será preciso registrá-la no Azure AD e configurá-la com permissões apropriadas. Isso habilitará o aplicativo a solicitar os tokens de acesso OAuth2 necessários para chamar a API. Para saber mais, confira [Introdução às APIs de Gerenciamento do Office 365](get-started-with-office-365-management-apis.md).

Para obter informações sobre os dados que a API da Atividade de Gerenciamento do Office 365 retorna, confira [Esquema da API da Atividade de Gerenciamento do Office 365](office-365-management-activity-api-schema.md).

> [!IMPORTANT]
> Para poder acessar dados por meio da API de Atividade de Gerenciamento do Office 365, habilite o log de auditoria unificado para a sua organização do Office 365. Para fazer isso, ative o log de auditoria do Office 365. Para obter instruções, confira [Ativar ou desativar a pesquisa de log de auditoria do Office 365](/microsoft-365/compliance/turn-audit-log-search-on-or-off).


## <a name="working-with-the-office-365-management-activity-api"></a>Usar a API da Atividade de Gerenciamento do Office 365

A API da Atividade de Gestão do Office 365 agrega ações e eventos em blobs de conteúdo específicos de locatário, que são classificados pelo tipo e pela origem de seu conteúdo. Atualmente, há suporte para estes tipos de conteúdo:

- Audit.AzureActiveDirectory
    
- Audit.Exchange
    
- Audit.SharePoint
    
- Audit.General (inclui todas as outras cargas de trabalho não incluídas nos tipos de conteúdo anteriores)

- DLP.All (apenas eventos DLP para todas as cargas de trabalho)
    
Para saber mais sobre os eventos e as propriedades associados a esses tipos de conteúdo, confira [Esquema de API da Atividade de Gerenciamento do Office 365](office-365-management-activity-api-schema.md).

Para começar a recuperar blobs de conteúdo para um locatário, primeiro você cria uma assinatura para os tipos de conteúdo desejados. Se está recuperando blobs de conteúdo para vários locatários, você cria várias assinaturas para cada um dos tipos de conteúdo desejados, uma para cada locatário.

Após criar uma assinatura, você pode realizar uma sondagem regularmente para descobrir novos blobs de conteúdo que estão disponíveis para download ou pode registrar um ponto de extremidade de webhook com a assinatura, e enviaremos notificações a esse ponto de extremidade quando novos blobs de conteúdo estiverem disponíveis.


> [!NOTE] 
> Quando uma assinatura é criada, pode levar até 12 horas para que os primeiros blobs de conteúdo fiquem disponíveis para essa assinatura. Os blobs de conteúdo são criados com a coleta e a agregação de ações e eventos entre vários servidores e data centers. Como resultado desse processo distribuído, as ações e os eventos contidos nos blobs de conteúdo não necessariamente aparecerão na ordem em que ocorreram. Um blob de conteúdo pode ter ações e eventos que ocorreram antes das ações e dos eventos contidos em um blob de conteúdo anterior. Estamos trabalhando para reduzir a latência entre a ocorrência de ações e eventos, bem como a respectiva disponibilidade em um blob de conteúdo, mas não podemos garantir que eles apareçam sequencialmente.


> [!NOTE] 
> Os dados confidenciais de DLP só estão disponíveis na API de feed de atividades para usuários que receberam permissões para "Ler dados confidenciais de DLP". Para saber mais sobre DLP (Prevenção contra Perda de Dados), confira [Visão geral de Políticas de Prevenção contra Perda de Dados](https://support.office.com/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)

## <a name="activity-api-operations"></a>Operações de API de atividade

Todas as operações da API têm como escopo um único locatário, e a URL raiz da API inclui uma ID de locatário que especifica o contexto do locatário. A ID de locatário é um GUID. Para saber mais sobre como obter o GUID, confira [Introdução às APIs de Gerenciamento do Office 365](get-started-with-office-365-management-apis.md). 

Como as notificações que enviamos a seu webhook incluem a ID de locatário, você pode usar o mesmo webhook para receber notificações para todos os locatários.

A URL do ponto de extremidade da API que você usa é baseada no tipo de plano de assinatura do Microsoft 365 ou Office 365 para sua organização.

**Plano empresarial**

```http
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**Plano governamental do GCC**

```http
https://manage-gcc.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**Plano de alto governo GCC**

```http
https://manage.office365.us/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**Plano governamental DoD**

```http
https://manage.protection.apps.mil/api/v1.0/{tenant_id}/activity/feed/{operation}
```

Todas as operações de API exigem um cabeçalho de Autorização HTTP com um token obtido do Azure AD. A ID de locatário no token de acesso deve corresponder à ID de locatário na URL raiz da API, e o token de acesso deve conter a declaração ActivityFeed.Read (isso corresponde à permissão [Ler dados de atividade de uma organização] que você configurou para o aplicativo no Azure AD).

```json
Authorization: Bearer eyJ0e...Qa6wg
```

A API de Atividade dá suporte às seguintes operações:

- **Iniciar uma assinatura** para começar a receber notificações e recuperar dados de atividade de um locatário.
    
- **Interromper uma assinatura** para descontinuar a recuperação de dados de um locatário.
    
- **Listar as assinaturas atuais**
    
- **Listar o conteúdo disponível** e as URLs de conteúdo correspondentes.
    
- **Receber notificações** enviadas por um webhook quando novo conteúdo está disponível.
    
- **Recuperar conteúdo** usando a URL de conteúdo.
    
- **Listar notificações** enviadas por um webhook.

- **Recuperar nomes amigáveis de recursos** para objetos no feed de dados identificados por guids.
    

## <a name="start-a-subscription"></a>Iniciar uma assinatura

Essa operação inicia uma assinatura para o tipo de conteúdo especificado. Se já existir uma assinatura para o tipo de conteúdo especificado, essa operação será usada para:

- Atualizar as propriedades de um webhook ativo.
    
- Habilitar um webhook que foi desabilitado devido a um número excessivo de notificações de falhas.
    
- Habilitar novamente um webhook expirado especificando uma data de término posterior ou nula.
    
- Remover um webhook.
    
||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/start?contentType={ContentType}`||
|**Parâmetros**|contentType|Deve ser um tipo de conteúdo válido.|
||PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
|**Body**|webhook|Objeto JSON opcional com três propriedades:<ul><li>**address**</b>: ponto de extremidade HTTPS necessário que pode receber notificações.  Uma mensagem de teste será enviada ao webhook para validá-lo antes da criação da assinatura.</li><li>**authId**: cadeia de caracteres opcional que é incluída como o cabeçalho WebHook AuthID na notificação enviada ao webhook para identificar e autorizar a origem da solicitação para o webhook.</li><li>**expiration**: data e hora opcionais que indicam uma data e uma hora após as quais as notificações não devem ser enviadas ao webhook.|
|**Response**|contentType|O tipo de conteúdo especificado na chamada.|
||status|O status da assinatura. Se uma assinatura estiver desabilitada, você não poderá listar ou recuperar conteúdo.|
||webhook|As propriedades de webhook especificadas na chamada, juntamente com o status do webhook. Se o webhook for desabilitado, você não receberá notificações, mas ainda poderá listar e recuperar conteúdo, desde que a assinatura esteja habilitada.|

#### <a name="sample-request"></a>Solicitação de amostra

```json
POST {root}/subscriptions/start?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Content-Type: application/json; utf-8
Authorization: Bearer eyJ0e...Qa6wg

{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }
}

```


#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "contentType": "Audit.SharePoint",
    "status": "enabled",
    "webhook": {
        "status": "enabled",
        "address":  "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": null
    }
}

```
## <a name="webhook-validation"></a>Validação de webhook

Quando a operação /start for chamada e um webhook for especificado, enviaremos uma notificação de validação ao endereço de webhook especificado para validar que um ouvinte ativo pode aceitar e processar notificações. Se não recebermos uma resposta HTTP 200 OK, a assinatura não será criada. Ou então, se /start for chamado para adicionar um webhook a uma assinatura existente, e uma resposta HTTP 200 OK não for recebida, o webhook não será adicionado, e a assinatura permanecerá inalterada.

#### <a name="sample-request"></a>Solicitação de amostra

```json
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId, if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```

#### <a name="sample-response"></a>Resposta de amostra

```json

HTTP/1.1 200 OK

```


## <a name="stop-a-subscription"></a>Interromper uma assinatura

Essa operação interrompe uma assinatura para o tipo de conteúdo especificado. 

Quando uma assinatura for interrompida, você não receberá mais notificações e não poderá recuperar o conteúdo disponível. Se a assinatura for reiniciada posteriormente, você terá acesso ao novo conteúdo desse ponto em diante. Você não poderá recuperar o conteúdo que estava disponível entre o momento em que a assinatura foi interrompida e reiniciada.


||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/stop?contentType={ContentType}`||
|**Parâmetros**|contentType|Deve ser um tipo de conteúdo válido.|
||PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
|**Body**|(vazio)||
|**Response**|(vazio)|||

#### <a name="sample-request"></a>Solicitação de amostra

```json
POST {root}/subscriptions/stop?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
```


## <a name="list-current-subscriptions"></a>Listar as assinaturas atuais

Essa operação retorna um conjunto de assinaturas atuais com os webhooks associados.

||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/list`||
|**Parâmetros**|PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
|**Body**|(vazio)||
|**Response**|Matriz JSON|Cada assinatura será representada por um objeto JSON com três propriedades:<ul><li>**contentType**: indica o tipo de conteúdo.</li><li>**status**: indica o status da assinatura.</li><li>**webhook**: indica o webhook configurado, juntamente com o status (habilitado, desabilitado, expirado) dele.  Se uma assinatura não tiver um webhook, a propriedade webhook estará presente, mas com um valor nulo.|


#### <a name="sample-request"></a>Solicitação de amostra

```json
GET {root}/subscriptions/list?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType" : "Audit.SharePoint",
        "status": "enabled",
        "webhook": {
            "status": "enabled",
            "address": "https://webhook.myapp.com/o365/",
            "authId": "o365activityapinotification",
            "expiration": null
        }
    },

    ...

    {
        "contentType": "Audit.Exchange",
        "webhook": null
    }
]

```


## <a name="list-available-content"></a>Listar conteúdo disponível

Essa operação lista o conteúdo atualmente disponível para recuperação para o tipo de conteúdo especificado. O conteúdo é uma agregação de ações e eventos coletados de vários servidores em vários data centers. O conteúdo será listado na ordem em que as agregações se tornarem disponíveis, mas não há garantia de que os eventos e as ações nas agregações sejam sequenciais. Um erro será retornado se o status da assinatura for desabilitado.


||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**Parâmetros**|contentType|Deve ser um tipo de conteúdo válido.|
||PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
||startTime endTime|As datas e horas opcionais (UTC) que indicam o intervalo de tempo do conteúdo a ser retornado, com base em quando o conteúdo foi disponibilizado. O intervalo de tempo é inclusivo em relação a startTime(startTime <= contentCreated) e exclusivo em relação a endTime (contentCreated < endTime). Portanto, intervalos de tempo incrementos e não sobrepostos podem ser usados para percorrer o conteúdo disponível.<ul><li>AAAA-MM-DD</li><li>AAAA-MM-DDTHH:MM</li><li>AAAA-MM-DDTHH:MM:SS</ul>Ambos devem ser especificados (ou ambos omitidos), não devem estar separados por mais de 24 horas e a hora de início não pode ser mais de sete dias no passado. Por padrão, se startTime e endTime forem omitidos, o conteúdo disponível nas últimas 24 horas será retornado.<p>**OBSERVAÇÃO**: embora seja possível especificar startTime e endTime separados por mais de 24 horas, isso não é recomendável. Além disso, se você obtiver resultados em resposta a uma solicitação de mais de 24 horas, serão resultados parciais e não deverão ser levados em consideração. A solicitação deve ser emitida com um intervalo de no máximo 24 horas entre startTime e endTime.</p>|
|**Response**|Matriz JSON|O conteúdo disponível será representado por objetos JSON com as seguintes propriedades:<ul><li>**contentType**: indica o tipo de conteúdo.</li><li>**contentId**: uma cadeia de caracteres opaca que identifica o conteúdo de forma exclusiva.</li><li> **contentUri**: a URL a ser usada ao recuperar o conteúdo.</li><li>**contentCreated**: data e hora em que o conteúdo foi disponibilizado.</li><li> **contentExpiration**: data e hora em que o conteúdo não estará mais disponível para recuperação.|


#### <a name="sample-request"></a>Solicitação de amostra

```json
GET {root}/subscriptions/content?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```

### <a name="pagination"></a>Paginação

Ao ser listado o conteúdo disponível para um intervalo de tempo, o número de resultados retornados é limitado para impedir tempos limite de resposta. Se houver mais resultados no intervalo de tempo especificado do que o máximo que pode ser retornados em uma única resposta, eles serão truncados, e um cabeçalho será adicionado à resposta para indicar a URL a ser usada para recuperar a próxima página de resultados. A URL conterá os mesmos parâmetros _startTime_ e _endTime_ especificados na solicitação original, juntamente com um parâmetro que indica a ID interna da próxima página. Se _startTime_ e _endTime_ não tiverem sido especificados na solicitação original, eles serão definidos para refletir o intervalo de 24 horas que precedia a solicitação original.


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

Para listar todo o conteúdo disponível para um intervalo de tempo especificado, talvez seja necessário recuperar várias páginas até que uma resposta sem o cabeçalho **NextPageUri** seja recebida.


## <a name="receiving-notifications"></a>Receber notificações

Notificações são enviadas ao webhook configurado para uma assinatura à medida que novo conteúdo fica disponível. Como a notificação inclui o identificador de locatário, você pode usar o mesmo webhook para receber notificações para todos os locatários para os quais tem assinaturas.

A notificação é criada como um HTTP POST sobre TLS (TLS 1.0 e versões posteriores) para o endereço do webhook especificado. Se a configuração do webhook inclui uma ID de autenticação, a enviamos como um cabeçalho HTTP: Webhook AuthID. Uma resposta diferente de HTTP 200 OK será considerada uma falha, e a notificação será tentada novamente. Você também pode configurar o webhook para exigir autenticação baseada em certificado de cliente, e faremos a autenticação usando o certificado manage.office.com.

O corpo da solicitação conterá uma matriz de um ou mais objetos JSON que representam os blobs de conteúdo disponíveis. O número de blobs de conteúdo em cada notificação é limitado para manter o tamanho da notificação relativamente pequeno. Como esse limite pode ser alterado, a implementação deve consultar o comprimento da matriz, em vez de esperar um tamanho fixo. Cada objeto incluirá as mesmas propriedades retornadas pela operação /content, juntamente com o GUID do locatário ao qual os dados pertencem e o GUID do aplicativo que criou as assinaturas. Isso permite que o webhook estabeleça o contexto quando está sendo usado com vários locatários e aplicativos.

- **tenantId**: o GUID do locatário ao qual o conteúdo pertence.

- **clientId**: o GUID do aplicativo que criou a assinatura.

- **contentType**: indica o tipo de conteúdo.

- **contentId**: uma cadeia de caracteres opaca que identifica o conteúdo de forma exclusiva.

- **contentUri**: a URL a ser usada ao recuperar o conteúdo.

- **contentCreated**: data e hora em que o conteúdo foi disponibilizado.

- **contentExpiration**: data e hora em que o conteúdo não estará mais disponível para recuperação.
    
A seguir há um exemplo de uma notificação.

```json
POST https://webhook.myapp.com/o365/ 
Content-Type: application/json; utf-8
Webhook-AuthID: o365activityapinotification

[
    {
        "tenantId": "{GUID}",
        "clientId": "{GUID}",
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```

## <a name="notification-failure-and-retry"></a>Falha de notificação e nova tentativa

O sistema de notificação envia notificações à medida que novo conteúdo é disponibilizado. Se encontramos um número excessivo de falhas ao enviar notificações, o mecanismo de novas tentativas aumentará exponencialmente o tempo entre as novas tentativas. Se continuarmos a encontrar falhas, nos reservaremos o direito de desabilitar o webhook e interromper completamente o envio de notificações. A operação /start pode ser usada para reabilitar um webhook desabilitado.

## <a name="retrieving-content"></a>Recuperar conteúdo

Para recuperar um blob de conteúdo, faça uma solicitação GET em relação ao URI de conteúdo correspondente que está incluído na lista de conteúdo disponível e nas notificações enviadas a um webhook. O conteúdo retornado será um conjunto de mais ações ou eventos no formato JSON.

#### <a name="sample-request"></a>Solicitação de amostra

```json
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "CreationTime": "2015-06-29T20:03:19",
        "Id": "80c76bd2-9d81-4c57-a97a-accfc3443dca",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "failed",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "ExtendedProperties": [
            {
                "Name": "LoginError",
                "Value": "-2147217390;PP_E_BAD_PASSWORD;The entered and stored passwords do not match."
            }
        ],
        "Client": "Exchange",
        "LoginStatus": -2147217390,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:03:34",
        "Id": "4e655d3f-35fa-42e0-b050-264b2d255c7a",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "success",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Client": "Exchange",
        "LoginStatus": 0,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:04:55",
        "Id": "b567caf0-088e-4c1c-a4ea-633a1e3d66c8",
        "Operation": "Add User.",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 8,
        "ResultStatus": "success",
        "UserKey": "1003BFFD8EC47CA6@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ObjectId": "user001@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Actor": [
            {
                "ID": "1cef1fdb-ff52-48c4-8e4e-dfb5ea83d357",
                "Type": 2
            },
            {
                "ID": "admin@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "1003BFFD8EC47CA6",
                "Type": 3
            }
        ],
        "ActorContextId": "41463f53-8812-40f4-890f-865bf6e35190",
        "InterSystemsId": "c2ced078-ad57-4079-a743-5c37f5284790",
        "IntraSystemId": "d1497f7e-15b4-49aa-83ad-11a17ca4a2f4",
        "Target": [
            {
                "ID": "user001@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "10037FFE91510806",
                "Type": 3
            }
        ],
        "TargetContextId": "41463f53-8812-40f4-890f-865bf6e35190"
    }
]

```

## <a name="list-notifications"></a>Listar notificações

Essa operação lista todas as tentativas de notificação para o tipo de conteúdo especificado. Se você não tiver incluído um webhook ao iniciar a assinatura para o tipo de conteúdo, não haverá notificações a serem recuperadas. Como repetimos as notificações em caso de falha, essa operação pode retornar várias notificações para o mesmo conteúdo. A ordem em que as notificações são enviadas não necessariamente corresponde à ordem em que o conteúdo foi disponibilizado (especialmente quando há falhas e repetições). 

Você pode usar essa operação para ajudar a investigar problemas relacionados a webhooks e notificações, mas não deve usá-la para determinar qual conteúdo está disponível atualmente para recuperação. Use a operação /content em vez disso. Retornaremos um erro se o status de assinatura for desabilitado.


||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**Parâmetros**|contentType|Deve ser um tipo de conteúdo válido.|
||PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
||startTime endTime|Datas e horas opcionais (UTC) que indicam o intervalo de tempo do conteúdo a ser retornado, com base em quando o conteúdo foi disponibilizado. O intervalo de tempo é inclusivo em relação a _startTime_ ( _startTime_ <= contentCreated) e exclusivo em relação a _endTime_ (_contentCreated_ < endTime). Portanto, intervalos de tempo incrementos e não sobrepostos podem ser usados para percorrer o conteúdo disponível.<ul><li>AAAA-MM-DD</li><li>AAAA-MM-DDTHH:MM</li><li>AAAA-MM-DDTHH:MM:SS</ul>Ambos devem ser especificados (ou ambos omitidos), não devem estar separados por mais de 24 horas e a hora de início não pode ser mais de sete dias no passado. Por padrão, se _startTime_ e _endTime_ forem omitidos, o conteúdo disponível nas últimas 24 horas será retornado.|
|**Response**|Matriz JSON|As notificações serão representadas por objetos JSON com as seguintes propriedades: <ul><li>**contentType**: indica o tipo de conteúdo.</li><li>**contentId**: uma cadeia de caracteres opaca que identifica o conteúdo de forma exclusiva.</li><li>**contentUri**: a URL a ser usada ao recuperar o conteúdo. </li><li>**contentCreated**: data e hora em que o conteúdo foi disponibilizado.</li><li>**contentExpiration**: data e hora em que o conteúdo não estará mais disponível para recuperação.</li><li>**contentCreated**: data e hora em que o conteúdo foi disponibilizado.</li><li>**notificationStatus**: indica o êxito ou a falha da tentativa de notificação.</li></ul>|

#### <a name="sample-request"></a>Solicitação de amostra

```json
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z",
        "notificationSent": "2015-05-23T17:36:00.000Z",
        "notificationStatus": "success"

    },
    ...
]

```


### <a name="pagination"></a>Paginação

Ao ser listado o histórico de notificações para um intervalo de tempo, o número de resultados retornados é limitado para impedir tempos limite de resposta. Se houver mais resultados no intervalo de tempo especificado do que o máximo que pode ser retornados em uma única resposta, eles serão truncados, e um cabeçalho será adicionado à resposta para indicar a URL a ser usada para recuperar a próxima página de resultados. A URL conterá os mesmos parâmetros _startTime_ e _endTime_ especificados na solicitação original, juntamente com um parâmetro que indica a ID interna da próxima página. Se _startTime_ e _endTime_ não tiverem sido especificados na solicitação original, eles serão definidos para refletir o intervalo de 24 horas que precedia a solicitação original.

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

Para listar todo o conteúdo disponível para um intervalo de tempo especificado, talvez seja necessário recuperar várias páginas até que uma resposta sem o cabeçalho **NextPageUri** seja recebida.

## <a name="retrieve-resource-friendly-names"></a>Recuperar nomes amigáveis de recursos

Essa operação recupera nomes amigáveis de recursos para objetos no feed de dados identificados por guids. No momento, "DlpSensitiveType" é o único objeto com suporte. 


||Assinatura|Descrição|
|:-----|:-----|:-----|
|**Path**| `/resources/dlpSensitiveTypes`||
|**Parâmetros**|PublisherIdentifier|O GUID de locatário de codificação de fornecedor para a API. Este **não** é o GUID de aplicativo nem o GUID do cliente que está usando o aplicativo, mas o GUID da empresa que está escrevendo o código. Este parâmetro é usado para limitar a taxa de solicitações. Verifique se este parâmetro foi especificado em todas as solicitações emitidas para obter uma cota dedicada. Todas as solicitações recebidas sem este parâmetro compartilharão a mesma cota.|
|**Cabeçalhos**|Accept-Language|Cabeçalho para especificar o idioma desejado para nomes traduzidos. Por exemplo, use "pt-BR" para português ou "es" para espanhol. O idioma padrão (en-US) será retornado se esse cabeçalho não estiver presente.|
|**Body**|(vazio)||
|**Response**|Matriz JSON|O conteúdo disponível será representado por objetos JSON com as seguintes propriedades:<ul><li>**ID**</b>: indica o guid do tipo de informações confidenciais.</li><li>**name**: o nome amigável do tipo de informações confidenciais.</p></li></ul>|

#### <a name="sample-request"></a>Solicitação de amostra

```json
GET {root}/resources/dlpSensitiveTypes?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

#### <a name="sample-response"></a>Resposta de amostra

```json
HTTP/1.1 200 OK

[
    {
        "id": "50842eb7-edc8-4019-85dd-5a5c1f2bb085",
        "name": "CreditCardNumber"
    }, 
    {
        "id": "0e9b3178-9678-47dd-a509-37222ca96b42",
        "name": "EUDebitCardNumber"
    }, 
    ...
    {
    }
]

```

## <a name="api-throttling"></a>Limitação de API

As organizações que acessam logs de auditoria por meio da API da Atividade de Gestão do Office 365 foram restritas pelos limites no nível do editor. Isso significa que, para um Publisher obter dados em nome de vários clientes, o limite foi compartilhado por todos esses clientes.

Estamos migrando do limite no nível do editor para um limite no nível do locatário. O resultado é que todas as organizações terão sua própria cota de largura de banda totalmente alocada para acessar os dados de auditoria. Todas as organizações alocam inicialmente uma linha de base de 2.000 solicitações por minuto. Esse não é um limite estático e predefinido, mas está modelado em uma combinação de fatores, incluindo o número de assentos da organização e que as organizações do Office 365 e do Microsoft 365 E5 terão aproximadamente o dobro de largura de banda quanto organizações que não são do E5. Também haverá limite máximo quanto à largura de banda para proteger a integridade do serviço.

Para saber mais, confira a seção "acesso de largura de banda alta à API da Atividade de Gestão do Office 365" em [Auditoria avançada no Microsoft 365](/microsoft-365/compliance/advanced-audit#high-bandwidth-access-to-the-office-365-management-activity-api).

> [!NOTE] 
> Embora cada locatário possa enviar inicialmente até 2 mil solicitações por minuto, a Microsoft não pode garantir uma taxa de resposta. A taxa de resposta depende de vários fatores, como o desempenho do sistema do cliente, a capacidade e a velocidade da rede. 

## <a name="errors"></a>Erros

Quando o serviço encontrar um erro, relatará o código de resposta do erro ao chamador, usando a sintaxe de código de erro HTTP padrão. . Informações adicionais são incluídas no corpo da chamada com falha como um único objeto JSON. Um exemplo de um corpo de erro JSON completo é mostrado abaixo: 

```json

{ 
    "error":{ 
        "code":"AF50000",
        "message": "An internal server error occurred. Retry the request."
    } 
}

```


|||
|:-----|:-----|
|Código|Mensagem|
|AF10001|O conjunto de permissões ({0}) enviadas na solicitação não incluía a permissão esperada **ActivityFeed.Read**.<br/><br/>{0} = o conjunto de permissões no token de acesso.</p></li></ul>|
|AF20001|Parâmetro ausente: {0}. <br/><br/>{0} = nome do parâmetro ausente.</p></li></ul>|
|AF20002|Tipo de parâmetro inválido: {0}. Tipo esperado: {1}<br/><br/>{0} = nome do parâmetro inválido.</p>{1} = tipo esperado (int, datetime, guid).</p></li></ul>|
|AF20003|A expiração de {0} fornecida está definida como data e hora anteriores.<br/><br/>{0} = a expiração passada na chamada à API.</p></li></ul>|
|AF20010|A ID de locatário passada na URL ({0}) não correspondia à ID de locatário passada no token de acesso ({1}).<br/><br/>{0} = ID de locatário passada na URL{1} = ID de locatário passada no token de acesso</p></li></ul>|
|AF20011|A ID de locatário especificada ({0}) não existe no sistema ou foi excluída. <br/><br/> {0} = ID de locatário passada na URL</p></li></ul>|
|AF20012|A ID de locatário especificada ({0}) está configurada incorretamente no sistema. <br/><br/>  {0} = ID de locatário passada na URL</p></li></ul>|
|AF20013|A ID de locatário passada na URL ({0}) não é um GUID válido.<br/><br/>   {0} = ID de locatário passada na URL</p></li></ul>|
|AF20020|O tipo de conteúdo especificado não é válido.|
|AF20021|O ponto de extremidade de webhook {{0}) não pôde ser validado. {1}<br/><br/>{0} = endereço de webhook.<br/><br/>{1} = "O ponto de extremidade não retornou HTTP 200." ou "O endereço deve começar com HTTPS."</p></li></ul>|
|AF20022|Nenhuma assinatura foi encontrada para o tipo de conteúdo específico.|
|AF20023|A assinatura foi desabilitada por {0}.<br/><br/>{0} = "administrador de locatário" ou "administrador de serviço"</p></li></ul>|
|AF20030|A hora de início e a hora de término devem ser especificadas (ou omitidas) e devem ser separadas por um valor menor ou igual a 24 horas, com a hora de início no máximo sete dias no passado.|
|AF20031|Entrada de NextPage inválida: {0}.<br/><br/>{0} = o indicador de próxima página passado na URL</p></li></ul>|
|AF20050|O conteúdo especificado ({0}) não existe.<br/><br/>{0} = id de recurso ou URL de recurso</p></li></ul>|
|AF20051|O conteúdo solicitado com a chave {0} já expirou. Conteúdo com mais de 7 dias não pode ser recuperado.<<br/><br/>  {0} = ID de recurso ou URL de recurso</p></li></ul>|
|AF20052|A ID de conteúdo {0} na URL é inválida.<br/><br/>{0} = id de recurso ou URL de recurso</p></li></ul>|
|AF20053|Apenas um idioma pode estar presente no cabeçalho Accept-Language.|
|AF20054|Sintaxe inválida no cabeçalho Accept-Language.|
|AF429|Muitas solicitações. Method={0}, PublisherId={1}<br/><br/>{0} = Método HTTP<br/><br/>{1} = GUID de locatário usado como PublisherIdentifier</p></li></ul>|
|AF50000|Erro interno. Repita a solicitação.|
|||
