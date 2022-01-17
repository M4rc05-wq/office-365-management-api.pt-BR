---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Service Communications API reference
title: Referência da API de Comunicações de Serviço do Office 365
description: 'Use esta API para acessar os seguintes dados: Obter Serviços, Obter Status Atual, Obter Status Histórico e Obter Mensagens.'
ms.ContentId: d0b9341a-b205-5442-1c20-8fb56407351d
ms.topic: reference (API)
ms.date: ''
ms.localizationpriority: high
ms.openlocfilehash: 735a238b464d39eaaf1eaeade41d67bcb8a2459b
ms.sourcegitcommit: 3f0410d4915b1dcc8872b1cbbe05cb265eea87ff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/15/2022
ms.locfileid: "62054727"
---
# <a name="office-365-service-communications-api-reference"></a>Referência da API de Comunicações de Serviço do Office 365

> [!IMPORTANT]
> A versão herdada da API de comunicações de serviço documentada neste artigo foi desativada. A API de integridade e comunicações do serviço no Microsoft Graph já está disponível e substitui a API de comunicações do serviço. Para obter mais informações sobre a nova API do Microsoft Graph, consulte [Visão geral para acessar a integridade do serviço e as comunicações por meio do Microsoft Graph](/graph/service-communications-concept-overview).

Você pode usar o API de Comunicações do Serviço do Office 365 V2 para acessar os seguintes dados:

- **Obter Serviços**: obtenha a lista de serviços assinados.

- **Obter Status Atual**: obtenha uma visualização em tempo real dos eventos de manutenção e dos incidentes de serviço.

- **Obter Status Histórico**: obtenha uma visualização do histórico de incidentes de serviço.

- **Obter Mensagens**: comunicações do Localizar Incidente e Centro de Mensagens.

Atualmente, a API de Comunicações do Serviço do Office 365 contém dados para os serviços de nuvem do Office 365, Yammer, Dynamics CRM e Microsoft Intune.

## <a name="the-fundamentals"></a>Os conceitos básicos

A URL raiz da API inclui um identificador de locatário que atribui operações a um único locatário:

```http
https://manage.office.com/api/v1.0/{tenant_identifier}/ServiceComms/{operation}
```

A **API de Comunicações do Serviço do Office 365** é um serviço do REST que permite desenvolver soluções usando qualquer linguagem da web e ambiente de hospedagem compatível com os certificados HTTPS e X.509. A API depende do **Microsoft Azure Active Directory** e do protocolo **OAuth2** para autorização e autenticação. Para acessar a API do seu aplicativo, é preciso primeiro registrá-la no Azure AD e configurá-la com permissões no escopo apropriado. Isso permitirá que seu aplicativo solicite os tokens de acesso do OAuth2 necessários para chamar a API. Saiba mais sobre como registrar e configurar um aplicativo no Azure AD em [Introdução às APIs de Gerenciamento do Office 365](get-started-with-office-365-management-apis.md).

Todas as solicitações de API exigem um cabeçalho HTTP de Autorização com um token de acesso de JWT do OAuth2 obtido do Azure AD contendo a solicitação **ServiceHealth.Read** e o identificador do locatário deve corresponder ao identificador do locatário na URL raiz.

```json
Authorization: Bearer {OAuth2 token}
```

### <a name="request-headers"></a>Cabeçalhos de solicitação

Estes são os cabeçalhos de solicitação suportados para todas as operações da API de Comunicações do Serviço do Office 365.

|Cabeçalho|Descrição|
|:-----|:-----|
|**Aceitação (opcional)**|Estas são representações aceitas da resposta:<br/>**application/json;odata.metadata=full**<br/>**application/json;odata.metadata=minimal**<br/>[O padrão se o cabeçalho não for especificado] **application/json;odata.metadata=none**|
|**Autorização (obrigatória)**|Token de autorização (Token do Azure AD do JWT do portador) para a solicitação.|

<br/>

### <a name="response-headers"></a>Cabeçalhos de resposta

Estes são os cabeçalhos de respostas retornados para todas as operações da API de Comunicações do Serviço do Office 365:

|Cabeçalho|Descrição|
|:-----|:-----|
|**Content-Length**|O comprimento do corpo da resposta.|
|**Content-Type**|Representação da resposta:<br/>**application/json**<br/>**application/json;odata.metadata=full**<br/>**application/json;odata.metadata=minimal**<br/>**application/json;odata.metadata=none**<br/>**odata.streaming=true**|
|**Cache-Control**|Usado para especificar as diretivas que todos os mecanismos de cache devem seguir ao longo da cadeia de solicitação/resposta.|
|**Pragma**|Comportamentos específicos de implementação.|
|**Expires**|Quando o cliente deve fazer o recurso expirar.|
|**X-Activity-Id**|O ID de atividade gerado pelo servidor.|
|**OData-Version**|O OData Versão (4.0) com suporte.|
|**Date**|A data em UTC de quando a resposta foi enviada do servidor.|
|**X-Time-Taken**|O tempo que foi necessário para gerar a resposta (ms).|
|**X-Instance-Name**|O identificador da instância do Azure usado para gerar a resposta (para fins de depuração).|
|**Server**|O servidor usado para gerar a resposta (para fins de depuração).|
|**X-ASPNET-Version**|A versão do ASP.Net usada pelo servidor que gerou a resposta (para fins de depuração).|
|**X-Powered-By**|As tecnologias usadas no servidor que gerou a resposta (para fins de depuração).|
|||

<br/>

Estas são as operações da API de Comunicações do Serviço do Office 365.

## <a name="get-services"></a>Obter Serviços

Retorna a lista de serviços assinados.

|Informações|Fornecer manutenção|Descrição|
|:-----|:-----|:-----|
|**Caminho**| `/Services`||
|**Query-option**|$select|Escolha um subconjunto de propriedades.|
|**Resposta**|Lista de entidades "Service"|A entidade "Service" contém "Id" (cadeia de caracteres), "DisplayName" (cadeia de caracteres) e "FeatureNames" (lista de cadeias de caracteres).|
||||

### <a name="sample-request"></a>Solicitação de amostra

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Services 
Authorization: Bearer {AAD_Bearer_JWT_Token}

```

### <a name="sample-response"></a>Resposta de amostra

```json
{
    "value": [
        {
            "Id": "Exchange",
            "DisplayName": "Exchange Online",
            "FeatureNames": [
                "Sign-in",
                "E-Mail and calendar access",
                "E-Mail timely delivery",
                "Management and Provisioning",
                "Voice mail"
            ]
        },
        {
            "Id": "Lync",
            "DisplayName": "Lync Online",
            "FeatureNames": [
                "Audio and Video",
                "Federation",
                "Management and Provisioning",
                "Sign-In",
                "All Features",
                "Dial-In Conferencing",
                "Online Meetings",
                "Instant Messaging",
                "Presence",
                "Mobility"
            ]
        }
    ]
}
```

## <a name="get-current-status"></a>Obter Status Atual

Retorna o status do serviço das últimas 24 horas.

> [!NOTE] 
> A resposta do serviço conterá o status e quaisquer incidentes ocorridos dentro das últimas 24 horas. O valor StatusDate ou StatusTime retornado estará exatamente 24 horas no passado. Para obter a última atualização para um determinado incidente, use a funcionalidade de Obter Mensagens e leia o valor de LastUpdatedTime no registro de resposta que corresponde à ID do incidente. <br/>

<br/>

|Informações|Fornecer manutenção|Descrição|
|:-----|:-----|:-----|
|**Caminho**| `/CurrentStatus`||
|**Filtro**|Carga de trabalho|Filtrar por carga de trabalho (cadeia de caracteres, padrão: todos).|
|**Query-option**|$select|Escolha um subconjunto de propriedades.|
|**Resposta**|Lista de entidades "WorkloadStatus".|A entidade "WorkloadStatus" contém "Id" (cadeia de caracteres), "Workload" (cadeia de caracteres), "StatusTime"(DateTimeOffset), "WorkloadDisplayName" (cadeia de caracteres), "Status" (cadeia de caracteres), "IncidentIds" (lista de cadeias de caracteres) e FeatureGroupStatusCollection (lista de "FeatureStatus").<br/><br/>A entidade "FeatureStatus" contém "Feature" (cadeia de caracteres), "FeatureGroupDisplayName" (cadeia de cadeia de caracteres) e "FeatureStatus" (cadeia de caracteres).|
||||

### <a name="sample-request"></a>Solicitação de amostra

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/CurrentStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>Resposta de amostra

```json
{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Lync",
            "Workload": "Lync",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Lync Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "AudioVideo",
                    "FeatureGroupDisplayName": "Audio and Video",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Federation",
                    "FeatureGroupDisplayName": "Federation",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "ManagementandProvisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Sign-In",
                    "FeatureGroupDisplayName": "Sign-In",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "All",
                    "FeatureGroupDisplayName": "All Features",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "DialInConferencing",
                    "FeatureGroupDisplayName": "Dial-In Conferencing",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "OnlineMeetings",
                    "FeatureGroupDisplayName": "Online Meetings",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "InstantMessaging",
                    "FeatureGroupDisplayName": "Instant Messaging",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Presence",
                    "FeatureGroupDisplayName": "Presence",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Mobility",
                    "FeatureGroupDisplayName": "Mobility",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        }
    ]
}
```

### <a name="status-definitions"></a>Definições de status

As definições de status incluem os seguintes valores:

- Investigando
- ServiceDegradation
- ServiceInterruption
- RestoringService
- ExtendedRecovery
- InvestigationSuspended
- ServiceRestored
- FalsePositive
- PostIncidentReportPublished
- ServiceOperational

Para uma descrição dessas definições de status, consulte [Como verificar a saúde dos serviços do Microsoft 365](/enterprise/view-service-health#status-definitions).

## <a name="get-historical-status"></a>Obter Status Histórico

Retorna o status histórico do serviço, por dia, durante um determinado intervalo.

|Informações|Fornecer manutenção|Descrição|
|:-----|:-----|:-----|
|**Caminho**| `/HistoricalStatus`||
|**Filtros**|Carga de trabalho|Filtrar por carga de trabalho (cadeia de caracteres, padrão: todos).|
||StatusTime|Filtrar por dias maiores que StatusTime (DateTimeOffset, padrão: ge CurrentTime, 7 dias).|
|**Query-option**|$select|Escolha um subconjunto de propriedades.|
|**Resposta**|Lista de entidades "WorkloadStatus".|A entidade "WorkloadStatus" contém "Id" (cadeia de caracteres), "Workload" (cadeia de caracteres), "StatusTime"(DateTimeOffset), "WorkloadDisplayName" (cadeia de caracteres), "Status" (cadeia de caracteres), "IncidentIds" (lista de cadeias de caracteres) e FeatureGroupStatusCollection (lista de "FeatureStatus").<br/><br/>A entidade "FeatureStatus" contém "Feature" (cadeia de caracteres), "FeatureGroupDisplayName" (cadeia de cadeia de caracteres) e "FeatureStatus" (cadeia de caracteres).|
||||

### <a name="sample-request"></a>Solicitação de amostra

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/HistoricalStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>Resposta de amostra

```json
{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-10T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "InformationAvailable",
            "IncidentIds": [
                "EX20870"
            ],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "InformationAvailable"
                }
            ]
        }
    ]
}
```

## <a name="get-messages"></a>Obter Mensagens

Retorna as mensagens sobre o serviço de um determinado intervalo de tempo. Use o tipo de filtro para filtrar as mensagens por "Centro de Mensagens", "Manutenção Planejada" ou "Incidente de Serviço".

|Informações|Fornecer manutenção|Descrição|
|:-----|:-----|:-----|
|**Caminho**| `/Messages`||
|**Filtros**|Carga de trabalho|Filtrar por carga de trabalho (cadeia de caracteres, padrão: todos).|
||StartTime|Filtrar por Hora de Início (DateTimeOffset, padrão: ge CurrentTime – 7 dias).|
||EndTime|Filtrar por Hora de Término (DateTimeOffset, padrão: le CurrentTime).|
||MessageType|Filtrar por Tipo de Mensagem (Cadeia de caracteres, padrão: todos).|
||Id|Filtrar por Id (Cadeia de caracteres, padrão: todos).|
|**Query-option**|$select|Escolha um subconjunto de propriedades.|
||$top|Escolha o número máximo de resultados (padrão e máx. $top=100).|
||$skip|Ignorar o número de resultados (padrão: $skip=0).|
|**Resposta**|Lista de entidades de "Mensagem".|A entidade "Message" contém "Id" (cadeia de caracteres), "StartTime" (DateTimeOffset), "EndTime" (DateTimeOffset), "Status" (cadeia de caracteres), "Messages" (lista da entidade "MessagHistory"), "LastUpdatedTime" (DateTimeOffset), "Workload" (cadeia de caracteres), "WorkloadDisplayName" (cadeia de caracteres), "Feature" (cadeia de caracteres), "FeatureDisplayName" (cadeia de caracteres), "MessageType" (Enum, padrão: todos).<br/><br/>A entidade "MessageHistory" contém "PublishedTime" (DateTimeOffset), "MessageText" (cadeia de caracteres).|
||||

### <a name="sample-request"></a>Solicitação de amostra

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Messages
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>Resposta de amostra

```json
{
    "value": [
        {
            "Id": "EX20119",
            "Name": "EX20119",
            "Title": null,
            "StartTime": "2015-04-01T22:25:00Z",
            "EndTime": "2015-04-07T21:48:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-01T22:34:28.76Z",
                    "MessageText": "Current Status: Engineers are investigating an issue in which some customers may be experiencing problems accessing or using Exchange Online services or features. This event is actively being investigated. More information will be provided shortly."
                },
                {
                    "PublishedTime": "2015-04-03T18:45:32.56Z",
                    "MessageText": "Current Status: Engineers are implementing a fix within the affected infrastructure to remediate Exchange Online archive access issues. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client.\n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \n\nNext Update by: Monday, April 6, 2015, at 9:00 PM UTC\n"
                },
                {
                    "PublishedTime": "2015-04-06T21:17:34.007Z",
                    "MessageText": "Current Status: Deployment of the fix is almost complete. Engineers are monitoring service health to ensure the issue has been remediated. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client. \n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues. \n\nNext Update by: Tuesday, April 7, 2015, at 10:00 PM UTC "
                },
                {
                    "PublishedTime": "2015-04-08T21:54:49.06Z",
                    "MessageText": "Final Status: Engineers have implemented a fix which remediated end-user impact. \r\n\r\nUser Experience: Affected users were unable to access online archives when using Outlook Web App (OWA).\r\n\r\nCustomer Impact: Customer impact appears to have been limited. This issue only affected hybrid customers.\r\n\r\nIncident Start Time: Monday, March 30, 2015, at 9:28 AM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 8:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the monitoring infrastructure for improvements as this event was reported by customers before an alert prompted a high priority investigation. \r\n\r\nA post-incident report will be available on the Service Health Dashboard within five business days."
                }
            ],
            "LastUpdatedTime": "2015-04-08T21:54:49.55Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        },
        {
            "Id": "EX20789",
            "Name": "EX20789",
            "Title": null,
            "StartTime": "2015-04-06T20:00:00Z",
            "EndTime": "2015-04-07T23:00:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-07T23:26:44.643Z",
                    "MessageText": "Final Status: Engineers have validated that the deployed fix has resolved the issue and that service is restored.\r\n\r\nUser Experience: Affected users were unable to send or receive email while using Exchange Web Services (EWS) on their mobile devices.\r\n\r\nCustomer Impact: Customer impact appeared to be limited during this event. This issue was only affecting customers that use third-party Mobile Device Management software from Good Technology. As a workaround to provide immediate relief from impact, engineers implemented a configuration change for customers who reported this issue to Microsoft Support.\r\n\r\nIncident Start Time: Wednesday, April 1, 2015, at 2:00 PM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 10:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing efforts to improve service resiliency, an update was deployed to the infrastructure that facilitates connections from multiple Exchange Online protocols to mailbox databases. The update, however, contained a code issue that caused connectivity issues in some scenarios. \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the infrastructure update process to prevent reoccurrences of this scenario.\r\n\r\nPlease consider this service notification the final update on the event."
                }
            ],
            "LastUpdatedTime": "2015-04-07T23:26:45.08Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        }
    ]
}
```

## <a name="errors"></a>Erros

Quando o serviço encontra um erro, ele relata o código de resposta do erro ao chamador, usando a sintaxe de código de erro HTTP padrão. Conforme descrito na [especificação OData V4](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html), as informações adicionais são incluídas no corpo da chamada que falhou como um único objeto JSON. Este é um exemplo de um corpo de erro JSON completo:


```json
{ 
    "error":{ 
        "code":"AF5000.  An internal server error occurred.",
        "message": "Retry the request." 
    } 
}
```
