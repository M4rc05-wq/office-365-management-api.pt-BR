---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management Activity API schema
title: Esquema da API da Atividade de Gerenciamento do Office 365
description: 'O esquema da API da Atividade de Gestão do Office 365 é fornecido como um serviço de dados em duas camadas: esquema comum e esquema específico do serviço.'
ms.ContentId: 1c2bf08c-4f3b-26c0-e1b2-90b190f641f5
ms.topic: reference (API)
ms.date: ''
ms.localizationpriority: high
ms.openlocfilehash: 632d40cdbcba9db8d960d088500968a34b135732
ms.sourcegitcommit: c10afafe7e219a4fdeffff12f1d65f3da0f8d5d5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2021
ms.locfileid: "60275442"
---
# <a name="office-365-management-activity-api-schema"></a>Esquema da API da Atividade de Gerenciamento do Office 365

O esquema da API da Atividade de Gerenciamento do Office 365 é fornecido como um serviço de dados em duas camadas:

- **Esquema comum**. A interface para acessar os principais conceitos de auditoria do Office 365, como Tipo de registro, Hora de criação, Tipo de usuário e Ação, além de fornecer dimensões principais (como ID de usuário), especificações do local (como endereço IP do cliente) e propriedades específicas do serviço (como ID de objeto). Ele estabelece exibições uniformes e consistentes para os usuários extraírem todos os dados de auditoria do Office 365 em algumas exibições de nível superior com os parâmetros apropriados e fornece um esquema fixo para todas as fontes de dados, o que reduz significativamente o custo do aprendizado. O esquema Comum é originado de dados de produto que pertencem a cada equipe de produto, como o Exchange, o SharePoint, o Azure Active Directory, o Yammer e o OneDrive for Business. O campo ID de objeto pode ser estendido por equipes de produtos do Microsoft 365 para adicionar propriedades específicas do serviço.

- **Esquema específico de serviço**. Criado sobre o esquema comum para fornecer um conjunto de atributos específicos do serviço do Microsoft 365; por exemplo, esquema do SharePoint, esquema do OneDrive for Business e esquema de administração do Exchange.

## <a name="office-365-management-api-schemas"></a>Esquemas da API de Gerenciamento do Office 365

Este artigo fornece os detalhes sobre o Esquema Comum, bem como esquemas específicos de serviço. A tabela a seguir descreve os esquemas disponíveis.

|Nome do esquema|Descrição|
|:-----|:-----|
|[Esquema Comum](#common-schema)|O modo de exibição para extrair o Tipo de registro, ID de usuário, IP do cliente, Tipo de usuário e Ação junto com as dimensões principais, como propriedades do usuário (como UserID), propriedades do local (como IP do cliente) e propriedades específicas do serviço (como ID de objeto).|
|[Esquema Base do SharePoint](#sharepoint-base-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria do SharePoint.|
|[Operações de Arquivos do SharePoint](#sharepoint-file-operations)|Estende o esquema Base do SharePoint com as propriedades específicas do acesso e manipulação de arquivos no SharePoint.|
|[Esquema de compartilhamento do SharePoint](#sharepoint-sharing-schema)|Estende o esquema Base do SharePoint com as propriedades específicas do compartilhamento de arquivos.|
|[Esquema do SharePoint](#sharepoint-schema)|Estende o esquema Base do SharePoint com as propriedades específicas do SharePoint, mas não está relacionado ao acesso e manipulação de arquivos.|
|[Esquema do Project](#project-schema)|Estende o esquema Base do SharePoint com as propriedades específicas do Project.|
|[Esquema de administração do Exchange](#exchange-admin-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria do Exchange.|
|[Esquema de caixa de correio do Exchange](#exchange-mailbox-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria da caixa de correio do Exchange.|
|[Esquema Base do Azure Active Directory](#azure-active-directory-base-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria do Azure Active Directory.|
|[Esquema de Logon da Conta do Azure Active Directory](#azure-active-directory-account-logon-schema)|Estende o esquema Base do Azure Active Directory com as propriedades específicas de todos os eventos de logon do Azure Active Directory.|
|[Esquema de logon seguro do STS do Active Directory do Azure](#azure-active-directory-secure-token-service-sts-logon-schema)|Estende o esquema de base do Azure Active Directory às propriedades específicas de todos os eventos de logon do Serviço de Token Seguro (STS) do Active Directory do Azure.|
|[Esquema do Azure Active Directory](#azure-active-directory-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria do Azure Active Directory.|
|[Esquema DLP](#dlp-schema)|Estende o esquema Comum com as propriedades específicas de Eventos de Prevenção Contra Perda de Dados.|
|[Esquema do Centro de Conformidade e Segurança](#security-and-compliance-center-schema)|Estende o esquema Comum com as propriedades específicas de todos os Eventos do Centro de Conformidade e Segurança.|
|[Esquema de Alertas de Conformidade e Segurança](#security-and-compliance-alerts-schema)|Estende o esquema Comum com as propriedades específicas de todos os alertas de conformidade e segurança do Office 365.|
|[Esquema do Yammer](#yammer-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos do Yammer.|
|[Esquema Base de Segurança do Data Center](#data-center-security-base-schema)|Estende o esquema Comum com as propriedades específicas de todos os dados de auditoria de segurança do data center.|
|[Esquema Cmdlet de Segurança do Data Center](#data-center-security-cmdlet-schema)|Estende o esquema Base de Segurança do Data Center com as propriedades específicas de todos os dados de auditoria do cmdlet de segurança do datacenter.|
|[Esquema do Microsoft Teams](#microsoft-teams-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos do Microsoft Teams.|
|[Microsoft Defender para Office 365 e esquema de Investigação e Resposta a Ameaças](#microsoft-defender-for-office-365-and-threat-investigation-and-response-schema)|Estende o esquema Comum com as propriedades específicas do Defender for Office 365 e investigação de ameaças e dados de resposta.|
|[Esquema de envio](#submission-schema)|Estende o Esquema Comum com as propriedades específicas para envios de usuário e administrador no Microsoft Defender para Office 365.|
|[Esquema de eventos de investigação e resposta automatizadas](#automated-investigation-and-response-events-in-office-365)|Estende o esquema Comum com as propriedades específicas para os eventos de investigação e resposta (AIR) automatizados do Office 365. Para ver um exemplo, consulte o [blog da Comunidade de Tecnologia: Melhore a eficácia do seu SOC com o Microsoft Defender para Office 365 e a API de gerenciamento do O365](https://techcommunity.microsoft.com/t5/microsoft-security-and/improve-the-effectiveness-of-your-soc-with-office-365-atp-and/ba-p/1525185).|
|[Esquema de eventos de higiene](#hygiene-events-schema)|Estende o esquema Comum com as propriedades específicas para eventos na Proteção do Exchange Online e no Microsoft Defender para Office 365.|
|[Esquema do Power BI](#power-bi-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos do Power BI.|
|[Esquema do Dynamics 365](#dynamics-365-schema)|Estende o esquema Comum com as propriedades específicas dos eventos do Dynamics 365.|
|[Esquema do Workplace Analytics](#workplace-analytics-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos do Microsoft Workplace Analytics.|
|[Esquema de quarentena](#quarantine-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos de quarentena.|
|[Esquema do Microsoft Forms](#microsoft-forms-schema)|Estende o esquema Comum com as propriedades específicas a todos os eventos do Microsoft Forms.|
|[Esquema de rótulos MIP](#mip-label-schema)|Estende o esquema comum com as propriedades específicas a rótulos de sensibilidade aplicados manualmente ou automaticamente às mensagens de email.|
|[Esquema de conformidade de comunicações do Exchange](#communication-compliance-exchange-schema)|Estende o esquema comum com as propriedades específicas para o modelo de linguagem ofensiva de conformidade de comunicações.|
|[Esquema de relatórios](#reports-schema)|Estende o esquema Comum com as propriedades específicas de todos os eventos de quarentena.|
|||

## <a name="common-schema"></a>Esquema Comum

**EntityType Name**: AuditRecord

|Parâmetro|Tipo|Obrigatório?|Descrição|
|:-----|:-----|:-----|:-----|
|Id|Combination GUIDEdm.Guid|Sim|Identificador exclusivo de um registro de auditoria.|
|RecordType|Self.[AuditLogRecordType](#auditlogrecordtype)|Sim|O tipo de operação indicado pelo registro. Confira a tabela [AuditLogRecordType](#auditlogrecordtype) para obter detalhes sobre os tipos de registros de log de auditoria.|
|CreationTime|Edm.Date|Sim|A data e hora no Tempo Universal Coordenado (UTC) de quando o usuário realizou a atividade.|
|Operation|Edm.String|Sim|O nome do usuário ou atividade administrativa. Para obter uma descrição das operações/atividades mais comuns, veja [Pesquisar o log de auditoria no Centro de Proteção do Office 365](https://go.microsoft.com/fwlink/p/?LinkId=708432). Para a atividade de administração do Exchange, essa propriedade identifica o nome do cmdlet que foi executado. Para eventos Dlp, as opções podem ser "DlpRuleMatch", "DlpRuleUndo" ou "DlpInfo", que são descritas em "Esquema DLP" abaixo.|
|OrganizationId|Edm.Guid|Sim|O GUID para o arrendatário do Office 365 de sua organização. Este valor será sempre o mesmo para sua organização, independentemente do serviço do Office 365 em que ele ocorra.|
|UserType|Self.[UserType](#user-type)|Sim|O tipo de usuário que executou a operação. Confira a tabela [UserType](#user-type) para detalhes sobre os tipos de usuários.|
|UserKey|Edm.String|Sim|Uma ID alternativa para o usuário identificado na propriedade UserId. Por exemplo, essa propriedade é preenchida com a ID exclusiva do passaporte (PUID) para eventos executados por usuários no SharePoint, no OneDrive for Business e no Exchange. Essa propriedade também pode especificar o mesmo valor da propriedade UserID para eventos que ocorrem em outros serviços e eventos executados por contas do sistema.|
|Workload|Edm.String|Não|O serviço do Office 365 em que a atividade ocorreu. 
|ResultStatus|Edm.String|Não|Indica se a ação (especificada na propriedade Operation) foi bem-sucedida ou não. Os valores possíveis são **Succeeded**, **PartiallySucceeded** ou **Failed**. Para a atividade de administração do Exchange, o valor é **Verdadeiro** ou **Falso**.<br/><br/>**Importante**: Cargas de trabalho diferentes podem substituir o valor da propriedade ResultStatus. Por exemplo, para eventos de logon do STS do Active Directory do Azure, um valor de **Sucesso** para ResultStatus indica apenas que a operação HTTP foi bem-sucedida; isso não significa que o logon foi bem-sucedido. Para determinar se o logon real foi bem-sucedido ou não, consulte a propriedade LogonError no [esquema de logon do STS do Azure Active Directory](#azure-active-directory-secure-token-service-sts-logon-schema). Se o logon falhar, o valor dessa propriedade conterá o motivo da falha na tentativa de logon. |
|ObjectId|Edm.string|Não|Para atividades do SharePoint e do OneDrive for Business, o nome do caminho completo do arquivo ou pasta acessado pelo usuário. Para o log de auditoria do administrador do Exchange, o nome do objeto que foi modificado pelo cmdlet.|
|UserId|Edm.string|Sim|O UPN (User Principal Name) do usuário que executou a ação (especificado na propriedade Operation) que resultou no registro sendo registrado; por exemplo, `my_name@my_domain_name`. Observe que os registros da atividade executada pelas contas do sistema (como SHAREPOINT\system ou NT AUTHORITY\SYSTEM) também são incluídos. No SharePoint, outro valor exibido na propriedade UserId é app@sharepoint. Isso indica que o "usuário" recebeu permissões no SharePoint para executar ações em toda a organização (como pesquisar em um site do SharePoint ou em uma conta do OneDrive) em nome de um usuário, administrador ou serviço. Para saber mais, confira o [app@sharepoint usuário nos registros de auditoria](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#the-appsharepoint-user-in-audit-records). |
|ClientIP|Edm.String|Sim|O endereço IP do dispositivo que foi usado quando a atividade foi registrada. O endereço IP é exibido em um formato de endereço IPv4 ou IPv6.<br/><br/>Para alguns serviços, o valor exibido nessa propriedade pode ser o endereço IP de um aplicativo confiável (por exemplo, Office em aplicativos Web) chamando o serviço em nome de um usuário e não o endereço IP do dispositivo usado por quem realizou a atividade. <br/><br/>Além disso, para eventos relacionados ao Azure Active Directory, o endereço IP não é registrado e o valor da propriedade ClientIP é `null`.|
|Escopo|Self.[AuditLogScope](#auditlogscope)|Não|Esse evento foi criado por um serviço hospedado do O365 ou por um servidor local? Os valores possíveis são **online** e **onprem**. Observe que o SharePoint é a única carga de trabalho enviando eventos do local para o O365 atualmente.|
|||||

### <a name="enum-auditlogrecordtype---type-edmint32"></a>Enumeração: AuditLogRecordType - Tipo: Edm.Int32

#### <a name="auditlogrecordtype"></a>AuditLogRecordType

|Valor|Nome do membro|Descrição|
|:-----|:-----|:-----|
|1|ExchangeAdmin|Eventos do log de auditoria do administrador do Exchange.|
|2|ExchangeItem|Eventos de um log de auditoria de caixa de correio do Exchange para ações executadas em um único item, como criar ou receber uma mensagem de email.|
|3|ExchangeItemGroup|Eventos de um log de auditoria de caixa de correio do Exchange para ações que podem ser executadas em vários itens, como mover ou excluir uma ou mais mensagens de email.|
|4 |SharePoint|Eventos do SharePoint.|
|6 |SharePointFileOperation|Eventos de operação de arquivos do SharePoint.|
|7 |OneDrive|Eventos do OneDrive for Business.|
|8 |AzureActiveDirectory|Eventos do Azure Active Directory.|
|9 |AzureActiveDirectoryAccountLogon|Eventos de logon do Azure Active Directory OrgId (descontinuando).|
|10 |DataCenterSecurityCmdlet|Eventos de cmdlet de segurança do Data Center.|
|11|ComplianceDLPSharePoint|Eventos de proteção contra perda de dados (DLP) no SharePoint e no OneDrive for Business.|
|13|ComplianceDLPExchange|Eventos de Proteção contra a Perda de Dados (DLP), quando configurados via Política DLP Unificada. Não há suporte para eventos de DLP com base em Regras de Transporte do Exchange.|
|14 |SharePointSharingOperation|Eventos de compartilhamento do SharePoint.|
|15 |AzureActiveDirectoryStsLogon|Eventos de logon do Serviço de Token Seguro (STS) no Azure Active Directory.|
|16|SkypeForBusinessPSTNUsage|Eventos da Rede Telefônica Pública Comutada (PSTN) do Skype for Business.|
|17 |SkypeForBusinessUsersBlocked|Eventos de usuário bloqueados do Skype for Business.|
|18 |SecurityComplianceCenterEOPCmdlet|Ações de administração do Centro de Conformidade e Segurança.|
|19|ExchangeAggregatedOperation|Eventos de auditoria de caixa de correio do Exchange agregado.|
|20|PowerBIAudit|Eventos do Power BI.|
| 21 |CRM|Eventos do Dynamics 365.|
|22|Yammer|Eventos do Yammer.|
|23|SkypeForBusinessCmdlets|Eventos do Skype for Business.|
|24|Descoberta|Eventos para atividades de Descoberta Eletrônica realizados executando pesquisas de conteúdo e gerenciando casos de Descoberta Eletrônica no Centro de Conformidade e Segurança.|
|25|MicrosoftTeams|Eventos do Microsoft Teams.|
|28|ThreatIntelligence|Eventos de phishing e malware da Proteção do Exchange Online e do Microsoft Defender para Office 365.|
|29|MailSubmission|Envio de eventos da Proteção do Exchange Online e do Microsoft Defender para Office 365.|
|30|MicrosoftFlow|Eventos do Microsoft Power Automate (anteriormente chamado de Microsoft Flow).|
|31|AeD|Eventos de Descoberta Eletrônica Avançada.|
|32|MicrosoftStream|Eventos do Microsoft Stream.|
|33|ComplianceDLPSharePointClassification|Eventos relacionados à classificação DLP no SharePoint.|
|34|ThreatFinder|Eventos relacionados à campanha do Microsoft Defender para Office 365.|
|35|Project|Eventos do Microsoft Project.|
|36|SharePointListOperation|Eventos da Lista do SharePoint.|
|37|SharePointCommentOperation|Eventos de comentário do Microsoft Office SharePoint Online.|
|38|DataGovernance|Eventos relacionados às políticas de retenção e rótulos de retenção no Centro de Conformidade e Segurança|
|39|Kaizala|Eventos do Kaizala.|
|40|SecurityComplianceAlerts|Sinais de alerta de conformidade e segurança.|
|41|ThreatIntelligenceUrl|Eventos de anulação de bloqueios e de tempo de bloqueio de links seguros a partir do Microsoft Defender para Office 365|
|42|SecurityComplianceInsights|Eventos relacionados à informações e relatórios no centro de segurança e conformidade do Office 365.|
|43|MIPLabel|Eventos relacionados à detecção no pipeline de transporte de mensagens de email que foram marcadas (manual ou automaticamente) com rótulos de confidencialidade. |
|44|WorkplaceAnalytics|Eventos do Workplace Analytics.|
|45|PowerAppsApp|Eventos dos Aplicativos de Energia.|
|46|PowerAppsPlan|Eventos de plano de assinatura para Power Apps. |
|47|ThreatIntelligenceAtpContent|Eventos de phishing e malware para arquivos no SharePoint, OneDrive for Business e Microsoft Teams do Microsoft Defender para Office 365.|
|48|LabelContentExplorer|Eventos relacionados ao [explorador de conteúdo de classificação de dados](/microsoft-365/compliance/data-classification-content-explorer).|
|49|TeamsHealthcare|Eventos relacionados ao [Aplicativo dos pacientes](/MicrosoftTeams/expand-teams-across-your-org/healthcare/patients-audit) no Microsoft Teams para Assistência Médica.|
|50|ExchangeItemAggregated|Eventos relacionados à [ação de auditoria da caixa de correio MailItemsAccessed](/microsoft-365/compliance/mailitemsaccessed-forensics-investigations).|
|51|HygieneEvent|Eventos relacionados à proteção contra spam de saída. |
|52|DataInsightsRestApiAudit|Informações sobre os dados de eventos da API REST.|
|53|InformationBarrierPolicyApplication|Eventos relacionados à aplicação de políticas de barreira de informação.|
|54|SharePointListItemOperation|Eventos de item de lista do SharePoint.|
|55|SharePointContentTypeOperation|Eventos do tipo de conteúdo de lista do SharePoint.|
|56|SharePointFieldOperation|Eventos de campo de lista do SharePoint.|
|57|MicrosoftTeamsAdmin|Eventos de administração do Teams.|
|58|HRSignal|Eventos relacionados a sinais de dados de RH que dão suporte à solução de gerenciamento de risco interno.|
|59|MicrosoftTeamsDevice|Eventos de dispositivo do Teams.|
|60|MicrosoftTeamsAnalytics|Eventos de análise do Teams.|
|61|InformationWorkerProtection|Eventos relacionados a alertas de usuários comprometidos.|
|62|Campanha|Eventos de campanha por email do Microsoft Defender para Office 365.|
|63|DLPEndpoint|Eventos Endpoint DLP.|
|64|AirInvestigation|Eventos de resposta automática de incidente (AIR).|
|65|Quarentena|Eventos de quarentena.|
|66|MicrosoftForms|Eventos do Microsoft Forms.|
|67|ApplicationAudit|Eventos de auditoria de aplicativos.|
|68|ComplianceSupervisionExchange|Eventos controlados pelo modelo de linguagem ofensivo de conformidade de comunicações.|
|69|CustomerKeyServiceEncryption|Eventos relacionados ao serviço de criptografia de chave do cliente.|
|70|OfficeNative|Eventos relacionados a rótulos de confidencialidade aplicados a documentos do Office.|
|71|MipAutoLabelSharePointItem|Eventos de rotulagem automática no Microsoft Office SharePoint Online.|
|72|MipAutoLabelSharePointPolicyLocation|Eventos de política de rotulagem automática no Microsoft Office SharePoint Online.|
|73|MicrosoftTeamsShifts|Eventos de Turnos do Teams.|
|75|MipAutoLabelExchangeItem|Eventos de rotulagem automática no Exchange.|
|76|CortanaBriefing|Resumo dos eventos por email.|
|77|Pesquisar|Eventos relacionados à realização de consultas de pesquisa no Microsoft Office SharePoint Online e no Exchange.|
|78|WDATPAlerts|Eventos relacionados a alertas gerados pelo Windows Defender para Endpoint.|
|82|SensitivityLabelPolicyMatch|Eventos gerados quando o arquivo rotulado com um rótulo de confidencialidade é aberto ou renomeado.|
|83|SensitivityLabelAction|Evento gerado quando rótulos de sensibilidade são aplicados, atualizados ou removidos de um arquivo.|
|84|SensitivityLabeledFileAction|Eventos gerados quando um arquivo rotulado com um rótulo de confidencialidade é aberto ou renomeado.|
|85|AttackSim|Eventos com simuladores de ataque.|
|86|AirManualInvestigation|Eventos relacionados a investigações manuais em investigação e resposta automatizada (AIR). |
|87|SecurityComplianceRBAC|Eventos do RBAC de segurança e conformidade.|
|88|UserTraining|Eventos de treinamento do simulador de ataque no Microsoft Defender para Office 365.|
|89|AirAdminActionInvestigation|Eventos relacionados a ações administrativas em  investigação e resposta Automatizada (AIR).|
|90|MSTIC|Eventos de inteligência de ameaças no Microsoft Defender para Office 365.|
|91|PhysicalBadgingSignal|Eventos relacionados a sinais físicos de identificação que são compatíveis com a solução de gerenciamento de risco interno.|
|93|AipDiscover|Eventos do verificador de Proteção de Informações do Azure (AIP).|
|94|AipSensitivityLabelAction|Eventos com etiqueta de sigilo AIP. |
|95|AipProtectionAction|Eventos de proteção AIP.|
|96|AipFileDeleted|Eventos de exclusão de arquivos AIP.|
|97|AipHeartBeat|Eventos de pulsação AIP.|
|98|MCASAlerts|Eventos correspondentes a alertas acionados pelo Microsoft Cloud App Security.|
|99|OnPremisesFileShareScannerDlp|Eventos relacionados à verificação de dados confidenciais em compartilhamentos de arquivos.|
|100|OnPremisesSharePointScannerDlp|Eventos relacionados à verificação de dados confidenciais no Microsoft Office SharePoint Online.|
|101|ExchangeSearch|Eventos relacionados ao uso do Outlook na Web (OWA) para pesquisar itens da caixa de correio.|
|102|SharePointSearch|Eventos relacionados à pesquisa no site inicial do Microsoft Office SharePoint Online de uma organização.|
|103|PrivacyInsights|Eventos de informações de privacidade.|
|105|MyAnalyticsSettings|Eventos do MyAnalytics.|
|106|SecurityComplianceUserChange|Eventos relacionados à modificação ou exclusão de um usuário.|
|107|ComplianceDLPExchangeClassification|Eventos de classificação do Exchange DLP.|
|109|MipExactDataMatch|Eventos de classificação Exact Data Match (EDM).|
|110|MSDEResponseActions|Eventos relacionados a ações de resposta no Microsoft Defender para Ponto de Extremidade.|
|111|MSDEGeneralSettings|Eventos relacionados a ações de configurações gerais no Microsoft Defender para Ponto de Extremidade.|
|112|MSDEIndicatorsSettings|Eventos relacionados a ações de indicador no Microsoft Defender para Ponto de Extremidade.|
|113|MS365DCustomDetection|Eventos relacionados a ações de detecção personalizadas no Microsoft 365 Defender.|
|114|MSDERolesSettings|Eventos relacionados a ações de funções no Microsoft Defender para Ponto de Extremidade.|
|147|CoreReportingSettings|Relatórios de eventos de configurações.|
||||

### <a name="enum-user-type---type-edmint32"></a>Enumeração: User Type - Tipo: Edm.Int32

#### <a name="user-type"></a>User Type

|Valor|Nome do membro|Descrição|
|:-----|:-----|:-----|
|0|Regular|Um usuário regular.|
|1|Reserved|Um usuário reservado.|
|2|Admin|Um administrador.|
|3|DcAdmin|Um operador de datacenter da Microsoft.|
|4 |System|Uma conta do sistema.|
|5|Application|Um aplicativo.|
|6 |ServicePrincipal|Uma entidade de serviço.|
|7 |CustomPolicy|Uma política personalizada.|
|8 |SystemPolicy|Uma política do sistema.|
||||

### <a name="enum-auditlogscope---type-edmint32"></a>Enumeração: AuditLogScope - Tipo: Edm.Int32

#### <a name="auditlogscope"></a>AuditLogScope

|Valor|Nome do membro|Descrição|
|:-----|:-----|:-----|
|0|Online|Este evento foi criado por um serviço hospedado no O365.|
|1|Onprem|Este evento foi criado por um servidor local.|
||||

## <a name="sharepoint-base-schema"></a>Esquema base do SharePoint

|**Parâmetro**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Site|Edm.Guid|Não|O GUID do site onde o arquivo ou pasta acessado pelo usuário está localizado.|
|ItemType|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[ItemType](#itemtype)"|Não|O tipo de objeto que foi acessado ou modificado. Confira a tabela [ItemType](#itemtype) para obter detalhes sobre os tipos de objetos.|
|EventSource|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[EventSource](#eventsource)"|Não|Identifica que um evento ocorreu no SharePoint. Valores possíveis são **SharePoint** ou **ObjectModel**.|
|SourceName|Edm.String|Não|A entidade que acionou a operação auditada. Valores possíveis são SharePoint ou **ObjectModel**.|
|UserAgent|Edm.String|Não|Informações sobre o cliente ou navegador do usuário. Esta informação é fornecida pelo cliente ou navegador.|
|MachineDomainInfo|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|Não|Informações sobre as operações de sincronização do dispositivo. Essas informações são relatadas apenas se estiverem presentes na solicitação.|
|MachineId|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|Não|Informações sobre as operações de sincronização do dispositivo. Essas informações são relatadas apenas se estiverem presentes na solicitação.|
|||||

### <a name="enum-itemtype---type-edmint32"></a>Enumeração: ItemType - Tipo: Edm.Int32

#### <a name="itemtype"></a>ItemType

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Invalid|O item não consiste em nenhum dos outros tipos de itens (listados nesta tabela).|
|1|File|O item é um arquivo.|
|5|Folder|O item é uma pasta.|
|6 |Web|O item é uma Web.|
|7 |Site|O item é um site.|
|8 |Tenant|O item é um locatário.|
|9 |DocumentLibrary|O item é uma biblioteca de documentos.|
|11|Page|O item é uma página.|
||||

### <a name="enum-eventsource---type-edmint32"></a>Enumeração: EventSource - Tipo: Edm.Int32

#### <a name="eventsource"></a>EventSource

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|SharePoint|A origem do evento é o SharePoint.|
|1|ObjectModel|A origem do evento é o ObjectModel.|
||||

### <a name="enum-sharepointauditoperation---type-edmint32"></a>Enumeração: SharePointAuditOperation - Tipo: Edm.Int32

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|AccessInvitationAccepted|O destinatário de um convite para exibir ou editar um arquivo compartilhado (ou pasta) acessou o arquivo compartilhado clicando no link do convite.|
|AccessInvitationCreated|O usuário envia um convite para outra pessoa (dentro ou fora de sua organização) para exibir ou editar um arquivo ou pasta compartilhada em um site do Microsoft Office SharePoint Online ou OneDrive for Business. Os detalhes da entrada do evento identificam o nome do arquivo que foi compartilhado, o usuário para o qual o convite foi enviado e o tipo de permissão de compartilhamento selecionada pela pessoa que enviou o convite.|
|AccessInvitationExpired|Um convite enviado a um usuário externo expirou. Por padrão, um convite enviado a um usuário fora de sua organização expira após sete dias se não for aceito.|
|AccessInvitationRevoked|O administrador do site ou proprietário de um site ou documento no Microsoft Office SharePoint Online ou OneDrive for Business retira um convite que foi enviado a um usuário fora da sua organização. Um convite só pode ser retirado antes de ser aceito.|
|AccessInvitationUpdated|O usuário que criou e enviou um convite a outra pessoa para exibir ou editar um arquivo (ou pasta) compartilhado em um site do SharePoint ou do OneDrive for Business reenvia o convite.|
|AccessRequestApproved|O administrador do site ou proprietário de um site ou documento no SharePoint ou no OneDrive for Business aprova uma solicitação do usuário para acessar o site ou documento.|
|AccessRequestCreated|O usuário solicita acesso a um site ou documento no SharePoint ou no OneDrive for Business que ele não tem permissão para acessar. |
|AccessRequestRejected|O administrador do site ou proprietário de um site ou documento no SharePoint recusa uma solicitação do usuário para acessar o site ou documento.|
|ActivationEnabled|Os usuários podem habilitar modelos de formulário para navegador que não contenham código de formulário, exijam confiança total, permitam a renderização em um dispositivo móvel ou usem uma conexão de dados gerenciada por um administrador de servidor.|
|AdministratorAddedToTermStore|Administrador do repositório de termos adicionado.|
|AdministratorDeletedFromTermStore|Administrador do repositório de termos excluído.|
|AllowGroupCreationSet|O administrador ou proprietário do site adiciona um nível de permissão a um site do SharePoint ou do OneDrive for Business que permite ao usuário designado criar um grupo para esse site.|
|AppCatalogCreated|Catálogo de aplicativos criado para disponibilizar aplicativos de negócios personalizados para o seu ambiente do SharePoint.|
|AuditPolicyRemoved|Política de Ciclo de Vida do Documento removida para um conjunto de sites.|
|AuditPolicyUpdate|Política de Ciclo de Vida do Documento atualizada para um conjunto de sites.|
|AzureStreamingEnabledSet|Um proprietário de portal de vídeo permitiu o streaming de vídeo do Azure.|
|CollaborationTypeModified|O tipo de colaboração permitido em sites (por exemplo, intranet, extranet ou público) foi modificado.|
|ConnectedSiteSettingModified|O usuário criou, modificou ou excluiu o link entre um projeto e um site de projeto ou o usuário modificou a configuração de sincronização no link do Project Web App.|
|CreateSSOApplication|Aplicativo de destino criado no Serviço de Repositório Seguro.|
|CustomFieldOrLookupTableCreated|O usuário criou um campo personalizado ou uma tabela/item de consulta no Project Web App.|
|CustomFieldOrLookupTableDeleted|O usuário excluiu um campo personalizado ou uma tabela/item de consulta no Project Web App.|
|CustomFieldOrLookupTableModified|O usuário modificou um campo personalizado ou uma tabela/item de consulta no Project Web App.|
|CustomizeExemptUsers|O administrador global personalizou a lista de agentes de usuário isentos no Centro de administração do SharePoint. Você pode especificar quais agentes de usuário serão isentos de receber uma página da Web inteira para indexar. Isso significa que quando um agente de usuário especificado como isento encontra um formulário do InfoPath, o formulário é retornado como um arquivo XML em vez de uma página da Web inteira. Isso torna a indexação de formulários do InfoPath mais rápida.|
|DefaultLanguageChangedInTermStore*|Configuração de idioma alterada no repositório de terminologia.|
|DelegateModified|O usuário criou ou modificou um representante de segurança no Project Web App.|
|DelegateRemoved|O usuário excluiu um representante de segurança no Project Web App.|
|DeleteSSOApplication|Um aplicativo SSO foi excluído.|
|eDiscoveryHoldApplied|Um Bloqueio In-loco foi colocado em uma fonte de conteúdo. Os Bloqueios In-loco são gerenciados usando um conjunto de sites de Descoberta Eletrônica (como o Centro de Descoberta Eletrônica) no SharePoint.|
|eDiscoveryHoldRemoved|Um Bloqueio In-loco foi removido de uma fonte de conteúdo. Os Bloqueios In-loco são gerenciados usando um conjunto de sites de Descoberta Eletrônica (como o Centro de Descoberta Eletrônica) no SharePoint.|
|eDiscoverySearchPerformed|Uma pesquisa de Descoberta Eletrônica foi realizada usando um conjunto de sites de Descoberta Eletrônica no SharePoint.|
|EngagementAccepted|O usuário aceita um compromisso de recurso no Project Web App.|
|EngagementModified|O usuário modifica um compromisso de recurso no Project Web App.|
|EngagementRejected|O usuário rejeita um compromisso de recurso no Project Web App.|
|EnterpriseCalendarModified|O usuário copia, modifica ou exclui um calendário da empresa no Project Web App.|
|EntityDeleted|O usuário exclui um quadro de horários no Project Web App.|
|EntityForceCheckedIn|O usuário força um check-in em um calendário, campo personalizado ou tabela de pesquisa no Project Web App.|
|ExemptUserAgentSet|O administrador global adiciona um agente do usuário à lista de agentes do usuário isentos no Centro de administração do SharePoint.|
|FileAccessed|A conta do usuário ou do sistema acessa um arquivo em um site do Microsoft Office SharePoint Online ou OneDrive for Business. As contas do sistema também podem gerar eventos FileAccessed.|
|FileCheckOutDiscarded|O usuário descarta (ou desfaz) um arquivo em check-out. Isso significa que todas as alterações que ele tiver feito nesse arquivo durante o estado de check-out serão descartados, e não salvas na versão do documento localizada na biblioteca de documentos.|
|FileCheckedIn|O usuário devolve um documento que retirou de uma biblioteca de documentos do SharePoint ou do OneDrive for Business.|
|FileCheckedOut|O usuário faz o check-out de um documento localizado em uma biblioteca de documentos do Microsoft Office SharePoint Online ou OneDrive for Business. Os usuários podem fazer check-out e fazer alterações em documentos que foram compartilhados com eles.|
|FileCopied|O usuário copia um documento de um site do Microsoft Office SharePoint Online ou OneDrive for Business. O arquivo copiado pode ser salvo em outra pasta do site.|
|FileDeleted|O usuário exclui um documento de um site do SharePoint ou do OneDrive for Business.|
|FileDeletedFirstStageRecycleBin|O usuário exclui um arquivo da lixeira em um site do SharePoint ou do OneDrive for Business.|
|FileDeletedSecondStageRecycleBin|O usuário exclui um arquivo da lixeira de segundo estágio em um site do SharePoint ou do OneDrive for Business.|
|FileDownloaded|O usuário faz download de um documento de um site do SharePoint ou do OneDrive for Business.|
|FileFetched|Este evento foi substituído pelo evento FileAccessed e foi descontinuado.|
|FileModified|O usuário ou a conta do sistema modifica o conteúdo ou as propriedades de um documento localizado em um site do SharePoint ou do OneDrive for Business.|
|FileMoved|O usuário move um documento de seu local atual em um site do SharePoint ou do OneDrive for Business para um novo local.|
|FilePreviewed|O usuário visualiza um documento em um site do SharePoint ou OneDrive for Business.|
|FileRenamed|O usuário renomeia um documento em um site do SharePoint ou OneDrive for Business.|
|FileRestored|O usuário restaura um documento da lixeira de um site do SharePoint ou OneDrive for Business. |
|FileSyncDownloadedFull|O usuário estabelece um relacionamento de sincronização e faz download dos arquivos com êxito pela primeira vez em seu computador a partir de uma biblioteca de documentos do SharePoint ou do OneDrive for Business.|
|FileSyncDownloadedPartial|O usuário baixa com sucesso quaisquer alterações em arquivos da biblioteca de documentos do Microsoft Office SharePoint Online ou OneDrive for Business. Esse evento indica que todas as alterações feitas nos arquivos da biblioteca de documentos foram baixadas para o computador do usuário. Apenas as alterações foram baixadas porque a biblioteca de documentos foi previamente baixada pelo usuário (conforme indicado pelo FileSyncDow|
|FileSyncUploadedFull|O usuário estabelece um relacionamento de sincronização e faz upload dos arquivos com êxito pela primeira vez do seu computador para uma biblioteca de documentos do SharePoint ou do OneDrive for Business.|
|FileSyncUploadedPartial|O usuário carrega com sucesso as alterações de arquivos em uma biblioteca de documentos do Microsoft Office SharePoint Online ou OneDrive for Business. Este evento indica que todas as alterações feitas na versão local de um arquivo de uma biblioteca de documentos foram carregadas com sucesso  para a biblioteca de documentos. Somente as alterações são descarregadas porque esses arquivos foram previamente carregados pelo usuário (conforme indicado pelo evento FileSyncUploadedFull).|
|FileUploaded|O usuário faz upload de um documento para uma pasta em um site do SharePoint ou do OneDrive for Business. |
|FileViewed|Este evento foi substituído pelo evento FileAccessed e foi descontinuado.|
|FolderCopied|O usuário copia uma pasta de um site do SharePoint ou do OneDrive for Business para outro local no SharePoint ou no OneDrive for Business.|
|FolderCreated|O usuário cria uma pasta em um site do SharePoint ou do OneDrive for Business.|
|FolderDeleted|O usuário exclui uma pasta de um site do SharePoint ou do OneDrive for Business.|
|FolderDeletedFirstStageRecycleBin|O usuário exclui uma pasta da lixeira em um site do SharePoint ou do OneDrive for Business.|
|FolderDeletedSecondStageRecycleBin|O usuário exclui uma pasta da lixeira de segundo estágio em um site do SharePoint ou do OneDrive for Business.|
|FolderModified|O usuário modifica uma pasta em um site do SharePoint ou do OneDrive for Business. Este evento inclui alterações de metadados da pasta, como tags e propriedades.|
|FolderMoved|O usuário move uma pasta de um site do SharePoint ou do OneDrive for Business.|
|FolderRenamed|O usuário renomeia uma pasta de um site do SharePoint ou do OneDrive for Business.|
|FolderRestored|O usuário restaura uma pasta da lixeira em um site do SharePoint ou do OneDrive for Business.|
|GroupAdded|O administrador ou proprietário do site cria um grupo para um site do Microsoft Office SharePoint Online ou OneDrive for Business ou executa uma tarefa que resulta na criação de um grupo. Por exemplo, na primeira vez que um usuário cria um link para compartilhar um arquivo, um grupo de sistema é adicionado ao site OneDrive for Business do usuário. Este evento também pode ser o resultado de um usuário criar um link com permissões de edição para um arquivo compartilhado.|
|GroupRemoved|O usuário exclui um grupo de um site do SharePoint ou do OneDrive for Business. |
|GroupUpdated|O administrador ou proprietário do site altera as configurações de um grupo para um site do Microsoft Office SharePoint Online ou OneDrive for Business. Isso pode incluir a alteração do nome do grupo, quem pode visualizar ou editar a associação ao grupo e como as solicitações de associação são tratadas.|
|LanguageAddedToTermStore|Idioma adicionado ao repositório de terminologia.|
|LanguageRemovedFromTermStore|Idioma removido do repositório de terminologia.|
|LegacyWorkflowEnabledSet|O administrador ou proprietário do site adiciona o tipo de conteúdo Tarefa de Fluxo de Trabalho do SharePoint ao site. Os administradores globais também podem ativar fluxos de trabalho para toda a organização no Centro de Administração do SharePoint.|
|LookAndFeelModified|O usuário modifica um início rápido, formatos de gráfico de gantt ou formatos de grupo.  Ou o usuário cria, modifica ou exclui uma exibição no Project Web App.|
|ManagedSyncClientAllowed|O usuário estabelece com êxito uma relação de sincronização com um site do SharePoint ou do OneDrive for Business. A relação de sincronização é bem-sucedida porque o computador do usuário é membro de um domínio que foi adicionado à lista de domínios (chamada de Lista de Destinatários Confiáveis) que pode acessar bibliotecas de documentos em sua organização. Para obter mais informações, confira [PowerShell do SharePoint Online ](https://go.microsoft.com/fwlink/p/?LinkID=534609) para habilitar a sincronização do OneDrive para domínios que estão na lista de destinatários confiáveis.|
|MaxQuotaModified|A cota máxima de um site foi modificada.|
|MaxResourceUsageModified|O uso máximo permitido de recursos para um site foi modificado.|
|MySitePublicEnabledSet|O sinalizador que permite aos usuários ter MySites públicos foi definido pelo Administrador de serviços do SharePoint.|
|NewsFeedEnabledSet|O administrador ou proprietário do site habilita feeds RSS para um site do SharePoint ou OneDrive for Business. Os administradores globais podem habilitar o RSS feeds para toda a organização no Centro de administração do SharePoint.|
|ODBNextUXSettings|Uma nova interface do usuário do OneDrive for Business foi ativada.|
|OfficeOnDemandSet|O administrador do site ativa o Office on Demand, que permite aos usuários acessar a versão mais recente dos aplicativos de área de trabalho do Office. O Office on Demand é habilitado no Centro de administração do SharePoint e requer uma assinatura do Office 365 que inclua aplicações completas e instaladas do Office.|
|PageViewed|O usuário visualiza uma página em um site do SharePoint ou no site do OneDrive for Business. Isso não inclui a exibição de arquivos da biblioteca de documentos de um site do SharePoint ou do site One Drive for Business em um navegador.|
|PeopleResultsScopeSet|O administrador do site cria ou altera a fonte de resultados para Pesquisas de Pessoas em um site do SharePoint.|
|PermissionSyncSettingModified|O usuário modifica as configurações de sincronização de permissão do projeto no Project Web App.|
|PermissionTemplateModified|O usuário cria, modifica ou exclui um modelo de permissões no Project Web App.|
|PortfolioDataAccessed|O usuário acessa o conteúdo do portfólio (biblioteca de driver, priorização de driver, análises de portfólio) no Project Web App.|
|PortfolioDataModified|O usuário cria, modifica ou exclui dados do portfólio (biblioteca de driver, priorização de driver, análises de portfólio) no Project Web App.|
|PreviewModeEnabledSet|O administrador do site habilita a visualização do documento para um site do SharePoint.|
|ProjectAccessed|O usuário acessa o conteúdo do projeto no Project Web App.|
|ProjectCheckedIn|O usuário faz check-in em um projeto que ele fez check-out de um Project Web App.|
|ProjectCheckedOut|O usuário faz check-out de um projeto localizado em um Project Web App. Os usuários podem fazer check-out e fazer alterações em projetos para os quais têm permissão para abrir.|
|ProjectCreated|O usuário cria um projeto no Project Web App.|
|ProjectDeleted|O usuário exclui um projeto no Project Web App.|
|ProjectForceCheckedIn|O usuário força um check-in em um projeto no Project Web App.|
|ProjectModified|O usuário modifica um projeto no Project Web App.|
|ProjectPublished|O usuário publica um projeto no Project Web App.|
|ProjectWorkflowRestarted|O usuário reinicia um fluxo de trabalho no Project Web App.|
|PWASettingsAccessed|O usuário acessa as configurações do Project Web App via CSOM.|
|PWASettingsModified|O usuário modifica a configuração do Project Web App.|
|QueueJobStateModified|O usuário cancela ou reinicia um trabalho de fila no Project Web App.|
|QuotaWarningEnabledModified|Aviso de cota de armazenamento modificada.|
|RenderingEnabled|Modelos de formulário habilitados para navegador serão processados ​​pelos serviços de formulários do InfoPath.|
|ReportingAccessed|O usuário acessou o ponto de extremidade de relatório no Project Web App.|
|ReportingSettingModified|O usuário modifica a configuração de relatório no Project Web App.|
|ResourceAccessed|O usuário acessa um conteúdo de recursos da empresa no Project Web App.|
|ResourceCheckedIn|O usuário faz check-in em um recurso da empresa que ele fez check-out de um Project Web App.|
|ResourceCheckedOut|O usuário faz check-out de um recurso da empresa localizado no Project Web App.|
|ResourceCreated|O usuário cria um recurso da empresa no Project Web App.|
|ResourceDeleted|O usuário exclui um recurso da empresa no Project Web App.|
|ResourceForceCheckedIn|O usuário força um check-in de um recurso da empresa no Project Web App.|
|ResourceModified|O usuário modifica um recurso da empresa no Project Web App.|
|ResourcePlanCheckedInOrOut|O usuário faz check-in ou check-out de um plano de recursos no Project Web App.|
|ResourcePlanModified|O usuário modifica um plano de recursos no Project Web App.|
|ResourcePlanPublished|O usuário publica um plano de recursos no Project Web App.|
|ResourceRedacted|O usuário cria um recurso corporativo que remove todas as informações pessoais no Project Web App.|
|ResourceWarningEnabledModified|Aviso de cota de recursos modificada.|
|SSOGroupCredentialsSet|Credenciais de grupo definidas no Serviço de Repositório Seguro.|
|SSOUserCredentialsSet|Credenciais de usuário definidas no Serviço de Repositório Seguro.|
|SearchCenterUrlSet|Conjunto de URLs do Centro de Pesquisa.|
|SecondaryMySiteOwnerSet|Um usuário adicionou um proprietário secundário ao seu MySite.|
|SecurityCategoryModified|O usuário cria, modifica ou exclui uma categoria de segurança no Project Web App.|
|SecurityGroupModified|O usuário cria, modifica ou exclui um grupo de segurança no Project Web App.|
|SendToConnectionAdded|O administrador global cria uma nova conexão Enviar para na página de Gerenciamento de registros no Centro de Administração do SharePoint. Uma conexão Enviar para especifica as configurações de um repositório de documentos ou de uma central de registros. Quando você cria uma conexão Enviar para, um Organizador de conteúdo pode enviar documentos para o local especificado.|
|SendToConnectionRemoved|O administrador global exclui uma conexão Enviar para na página Gerenciamento de registros no Centro de administração do SharePoint.|
|SharedLinkCreated|O usuário cria um link para um arquivo compartilhado no Microsoft Office SharePoint Online ou OneDrive for Business. Este link pode ser enviado a outras pessoas para lhes dar acesso ao arquivo. Um usuário pode criar dois tipos de links: um link que permite a um usuário visualizar e editar o arquivo compartilhado ou um link que permite ao usuário apenas visualizar o arquivo.|
|SharedLinkDisabled|O usuário desabilita (permanentemente) um link que foi criado para compartilhar um arquivo.|
|SharingInvitationAccepted *|O usuário aceita um convite para compartilhar um arquivo ou uma pasta. Esse evento é registrado quando um usuário compartilha um arquivo com outros usuários.|
|SharingRevoked|O usuário descompartilha um arquivo ou pasta anteriormente compartilhada com outros usuários. Esse evento é registrado quando um usuário deixa de compartilhar um arquivo com outros usuários.|
|SharingSet|O usuário compartilha um arquivo ou pasta localizado no SharePoint ou no OneDrive for Business com outro usuário dentro de sua organização.|
|SiteAdminChangeRequest|O usuário solicita a adição como administrador de conjunto de sites de um conjunto de sites do SharePoint. Os administradores de conjunto de sites têm permissões de controle total para o conjunto de sites e todos seus subsites.|
|SiteCollectionAdminAdded*|O administrador ou proprietário de um conjunto de sites adiciona uma pessoa como administrador do conjunto de sites para um site do Microsoft Office SharePoint Online ou OneDrive for Business. Os administradores do conjunto de sites têm permissões de controle total para o conjunto de sites e todos os subsites.|
|SiteCollectionCreated| O administrador global cria um novo conjunto de sites em sua organização do SharePoint.|
|SiteRenamed|O administrador ou proprietário do site renomeia um site do SharePoint ou do OneDrive for Business.|
|StatusReportModified|O usuário cria, modifica ou exclui um relatório de status no Project Web App.|
|SyncGetChanges|O usuário clica em **Sincronizar** na bandeja de ação no SharePoint ou no OneDrive for Business para sincronizar as alterações feitas no arquivo em uma biblioteca de documentos ao computador.|
|TaskStatusAccessed|O usuário acessa o status de uma ou mais tarefas no Project Web App.|
|TaskStatusApproved|O usuário aprova uma atualização de status de uma ou mais tarefas no Project Web App.|
|TaskStatusRejected|O usuário rejeita uma atualização de status de uma ou mais tarefas no Project Web App.|
|TaskStatusSaved|O usuário salva uma atualização de status de uma ou mais tarefas no Project Web App.|
|TaskStatusSubmitted|O usuário envia uma atualização de status de uma ou mais tarefas no Project Web App.|
|TimesheetAccessed|O usuário acessa um quadro de horários no Project Web App.|
|TimesheetApproved|O usuário aprova um quadro de horários no Project Web App.|
|TimesheetRejected|O usuário rejeita um quadro de horários no Project Web App.|
|TimesheetSaved|O usuário salva um quadro de horários no Project Web App.|
|TimesheetSubmitted|O usuário envia um quadro de horários de status no Project Web App.|
|UnmanagedSyncClientBlocked|O usuário tenta estabelecer uma relação de sincronização com um site do SharePoint ou OneDrive for Business a partir de um computador que não é membro do domínio de sua organização ou é membro de um domínio que não foi adicionado à lista de domínios (chamado de seguro lista de destinatários) que podem acessar bibliotecas de documentos em sua organização. A relação de sincronização não é permitida e o computador do usuário está impedido de sincronizar, baixar ou carregar arquivos em uma biblioteca de documentos. Para obter informações sobre esse recurso, consulte [ Use cmdlets do Windows PowerShell para habilitar a sincronização do OneDrive para domínios que estão na lista de destinatários seguros](/powershell/module/sharepoint-online/index).|
|UpdateSSOApplication|Aplicativo de destino atualizado no Serviço de Repositório Seguro.|
|UserAddedToGroup|O administrador ou proprietário do site adiciona uma pessoa a um grupo em um site do Microsoft Office SharePoint Online ou OneDrive for Business. Adicionar uma pessoa a um grupo concede ao usuário as permissões que foram atribuídas ao grupo. |
|UserRemovedFromGroup|O administrador ou proprietário do site remove uma pessoa de um grupo em um site do Microsoft Office SharePoint Online ou OneDrive for Business. Depois que a pessoa é removida, não lhe são mais concedidas as permissões que foram atribuídas ao grupo. |
|WorkflowModified|O usuário cria, modifica ou exclui fases ou estágios do tipo de projeto ou fluxo de trabalho corporativo no Project Web App.|
|||||

## <a name="sharepoint-file-operations"></a>Operações de arquivos do SharePoint

Os eventos do SharePoint relacionados a arquivos listados na seção "Atividades de arquivos e pastas" em [Pesquisar o log de auditoria do centro de conformidade e segurança](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) usam este esquema.

|**Parâmetro**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|SiteUrl|Edm.String|Sim|A URL do site onde o arquivo ou pasta acessado pelo usuário está localizado.|
|SourceRelativeUrl|Edm.String|Não|O URL da pasta que contém o arquivo acessado pelo usuário. A combinação dos valores dos parâmetros _SiteURL_, _SourceRelativeURL_ e _SourceFileName_ é igual ao valor da propriedade **ObjectID**, que é o nome do caminho completo do arquivo acessado pelo usuário.|
|SourceFileName|Edm.String|Sim|O nome do arquivo ou pasta acessado pelo usuário.|
|SourceFileExtension|Edm.String|Não|A extensão do arquivo que foi acessado pelo usuário. Esta propriedade fica em branco se o objeto que foi acessado for uma pasta.|
|DestinationRelativeUrl|Edm.String|Não|A URL da pasta de destino em que um arquivo é copiado ou movido. A combinação dos valores para os parâmetros _SiteURL_, _DestinationRelativeURL_ e _DestinationFileName_ é igual ao valor da propriedade **ObjectID**, que é o nome do caminho completo para o arquivo que foi copiado. Essa propriedade é exibida apenas para eventos FileCopied e FileMoved.|
|DestinationFileName|Edm.String|Não|O nome do arquivo que foi copiado ou movido. Essa propriedade é exibida apenas para eventos FileCopied e FileMoved.|
|DestinationFileExtension|Edm.String|Não|A extensão de um arquivo que foi copiado ou movido. Essa propriedade é exibida apenas para eventos FileCopied e FileMoved.|
|UserSharedWith|Edm.String|Não|O usuário com o qual um recurso foi compartilhado.|
|SharingType|Edm.String|Não|O tipo de permissões de compartilhamento que foram designadas ao usuário com o qual o recurso foi compartilhado. Este usuário é identificado pelo parâmetro _UserSharedWith_.|
|||||

## <a name="sharepoint-sharing-schema"></a>Esquema de compartilhamento do SharePoint

 Os eventos do SharePoint relacionados ao compartilhamento de arquivos. Eles são diferentes dos eventos relacionados a arquivos e pastas em que um usuário está realizando uma ação que afeta outro usuário. Para obter informações sobre o esquema de compartilhamento do SharePoint, confira [Usar a auditoria de compartilhamento no log de auditoria do Office 365g](/microsoft-365/compliance/use-sharing-auditing
).

|**Parâmetro**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|TargetUserOrGroupName |Edm.String|Não|Armazena o UPN ou o nome do usuário ou grupo de destino com o qual um recurso foi compartilhado.|
|TargetUserOrGroupType|Edm.String|Não|Identifica se o usuário ou grupo de destino é um Membro, Convidado, Grupo ou Parceiro. |
|EventData|Código XML|Não|Transmite informações de acompanhamento sobre a ação de compartilhamento que ocorreu, como adicionar um usuário a um grupo ou conceder permissões de edição.|
|||||

## <a name="sharepoint-schema"></a>Esquema do SharePoint

Os eventos do SharePoint listados em [Pesquisar o log de auditoria do centro de conformidade e segurança](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) (excluindo os eventos de arquivo e pasta) usam este esquema.

|**Parâmetro**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|CustomEvent|Edm.String|Não|Cadeia de caracteres opcional para eventos personalizados.|
|EventData|Edm.String|Não|Carga opcional para eventos personalizados.|
|ModifiedProperties|Collection(ModifiedProperty)|Não|A propriedade está incluída para eventos de administrador, como adicionar um usuário como membro de um site ou grupo de administradores de um conjunto de sites. A propriedade inclui o nome da propriedade que foi modificada (por exemplo, o grupo Administrador do Site), o novo valor da propriedade modificada (como o usuário que foi adicionado como administrador do site) e o valor anterior do objeto modificado.|
|||||

## <a name="project-schema"></a>Esquema do Project

|**Parâmetro**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Entity|Edm.String|Sim| [ProjectEntity](#project-entity) da auditoria.|
|Action|Edm.String|Sim|[ProjectAction](#project-action) tomada.|
|OnBehalfOfResId|Edm.Guid|Não|A ID do recurso em nome da qual a ação foi tomada.|
|||||

### <a name="enum-project-action---type-edmint32"></a>Enumeração: Project Action - Tipo: Edm.Int32

#### <a name="project-action"></a>Ação do projeto

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|Accepted|O usuário aceitou um evento ou fluxo de trabalho.|
|Accessed|O usuário acessou uma entidade.|
|Activated|O usuário ativou uma entidade, evento ou fluxo de trabalho.|
|Cancelled|O usuário cancelou um evento ou fluxo de trabalho.|
|CheckedIn|O usuário fez check-in em uma entidade.|
|CheckedOut|O usuário fez check-out em uma entidade.|
|Copied|O usuário copiou uma entidade.|
|Created|O usuário criou uma entidade.|
|Deactivated|O usuário desativou uma entidade.|
|Deleted|O usuário excluiu uma entidade.|
|Exported|O usuário exportou uma entidade.|
|ForceCheckedIn|O usuário fez com que uma entidade fosse obrigada a fazer check-in.|
|Modified|O usuário modificou uma entidade.|
|Published|O usuário publicou uma entidade.|
|Redacted|O usuário redigiu uma entidade.|
|Rejected|O usuário rejeitou uma entidade.|
|Restarted|O usuário reiniciou um evento ou fluxo de trabalho.|
|Saved|O usuário salvou uma entidade.|
|Sent|O usuário enviou uma entidade.|
|Submitted|O usuário enviou uma entidade para revisão ou fluxo de trabalho.|
|||||

### <a name="enum-project-entity---type-edmint32"></a>Enumeração: Project Entity - Tipo: Edm.Int32

#### <a name="project-entity"></a>Entidade do projeto.

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|CustomField|Representa um campo personalizado da empresa.|
|Driver|Representa um indicador de portfólio.|
|DriverPrioritization|Representa uma priorização de portfólio.|
|Engagement|Representa um compromisso de recurso.|
|EnterpriseCalendar|Representa um calendário de recursos da empresa.|
|EnterpriseProjectType|Representa um tipo de projeto corporativo.|
|FiscalPeriod|Representa um período fiscal.|
|GanttChartFormat|Representa um formato de gráfico de Gantt.|
|GroupingFormat|Representa um formato de agrupamento de visualizações.|
|LineClassification|Representa uma classificação de linha de quadro de horários.|
|LookupTable|Representa uma tabela de pesquisa corporativa.|
|PermissionTemplate|Representa um modelo de permissão de segurança.|
|PortfolioAnalysis|Representa uma análise de portfólio.|
|Project|Representa um projeto.|
|QueueJob|Representa um trabalho em fila.|
|QuickLaunch|Representa um item de inicialização rápida.|
|Reporting|Representa o ponto de extremidade de relatório.|
|Resource|Representa um recurso da empresa.|
|ResourcePlan|Representa um plano de recursos associado a um projeto.|
|SecurityCategory|Representa uma categoria de segurança.|
|SecurityGroup|Representa um grupo de segurança.|
|Setting|Representa uma configuração do Project Web App.|
|Statusing|Representa uma atualização de status da tarefa.|
|StatusReport|Representa um relatório de status.|
|TimeReportingPeriod|Representa um período de tempo para um quadro de horários.|
|Timesheet|Representa uma entidade de quadro de horários.|
|TimesheetAuditLog|Representa um log de auditoria do quadro de horários.|
|TimesheetManager|Representa o gerente de um quadro de horários.|
|UserDelegate|Representa uma delegação de usuário para outro usuário.|
|View|Representa uma definição de exibição.|
|WorkflowPhase|Representa uma fase em um fluxo de trabalho.|
|WorkflowStage|Representa um estágio em um fluxo de trabalho.|
|||||

## <a name="exchange-admin-schema"></a>Esquema de administração do Exchange

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|ModifiedObjectResolvedName|Edm.String|Não|Esse é o nome amigável do objeto que foi modificado pelo cmdlet. Ele é registrado somente se o cmdlet modificar o objeto.|
|Parâmetros|Collection(Common.NameValuePair)|Não|O nome e o valor de todos os parâmetros que foram usados ​​com o cmdlet identificado na propriedade Operations.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|Não|A propriedade está incluída para eventos administrativos. A propriedade inclui o nome da propriedade que foi modificada, o novo valor da propriedade modificada e o valor anterior do objeto modificado.|
|ExternalAccess|Edm.Boolean|Sim|Especifica se o cmdlet foi executado por um usuário em sua organização, pela equipe de datacenters da Microsoft ou por uma conta de serviço do datacenter ou por um administrador delegado. O valor **Falso** indica que o cmdlet foi executado por alguém em sua organização. O valor **Verdadeiro** indica que o cmdlet foi executado pela equipe do datacenter, por uma conta de serviço do datacenter ou por um administrador delegado.|
|OriginatingServer|Edm.String|Não|O nome do servidor do qual o cmdlet foi executado.|
|OrganizationName|Edm.String|Não|O nome do locatário.|
|||||

## <a name="exchange-mailbox-schema"></a>Esquema de caixa de correio do Exchange

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|LogonType|Self.[LogonType](#logontype)|Não| Indica o tipo de usuário que acessou a caixa de correio e executou a operação que foi registrada.|
|InternalLogonType|Self.[LogonType](#logontype)|Não|Reservado para uso interno.|
|MailboxGuid|Edm.String|Não|O GUID do Exchange da caixa de correio que foi acessada.|
|MailboxOwnerUPN|Edm.String|Não|O endereço de email da pessoa que possui a caixa de correio que foi acessada.|
|MailboxOwnerSid|Edm.String|Não|O SID do proprietário da caixa de correio.|
|MailboxOwnerMasterAccountSid|Edm.String|Não|SID da conta principal da conta do proprietário da caixa de correio.|
|LogonUserSid|Edm.String|Não|O SID do usuário que executou a operação.|
|LogonUserDisplayName|Edm.String|Não|O nome amigável do usuário que executou a operação.|
|ExternalAccess|Edm.Boolean|Sim|Isso se aplica se o domínio do usuário de logon for diferente do domínio do proprietário da caixa de correio.|
|OriginatingServer |Edm.String|Não|De onde a operação se originou.|
|OrganizationName|Edm.String|Não|O nome do locatário.|
|ClientInfoString|Edm.String|Não|Informações sobre o cliente de email que foi usado para executar a operação, como uma versão do navegador, versão do Outlook e informações do dispositivo móvel.|
|ClientIPAddress|Edm.String|Não|O endereço IP do dispositivo que foi usado quando a operação foi registrada. O endereço IP é exibido em um formato de endereço IPv4 ou IPv6.|
|ClientMachineName|Edm.String|Não|O nome da computador que hospeda o cliente do Outlook.|
|ClientProcessName|Edm.String|Não|O cliente de email que foi usado para acessar a caixa de correio. |
|ClientVersion|Edm.String|Não|A versão do cliente de email.|
|||||

### <a name="enum-logontype---type-edmint32"></a>Enumeração: LogonType - Tipo: Edm.Int32

#### <a name="logontype"></a>LogonType

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Owner|O proprietário da caixa de correio.|
|1|Admin|Uma pessoa com privilégios administrativos para a caixa de correio de uma pessoa.|
|2|Delegated|Uma pessoa com privilégios de delegado para a caixa de correio de uma pessoa.|
|3|Transport|Um serviço de transporte no datacenter da Microsoft.|
|4 |SystemService|Uma conta de serviço no datacenter da Microsoft.|
|5|BestAccess|Reservado para uso interno.|
|6 |DelegatedAdmin|Um administrador delegado.|
|||||

### <a name="exchangemailboxauditgrouprecord-schema"></a>Esquema ExchangeMailboxAuditGroupRecord

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Folder|Self.[ExchangeFolder](#exchangefolder-complex-type)|Não|A pasta onde um grupo de itens está localizado.|
|CrossMailboxOperations|Edm.Boolean|Não|Indica se a operação envolveu mais de uma caixa de correio.|
|DestMailboxId|Edm.Guid|Não|Definido somente se o parâmetro CrossMailboxOperations for **True**. Especifica o GUID da caixa de correio de destino.|
|DestMailboxOwnerUPN|Edm.String|Não|Definido somente se o parâmetro CrossMailboxOperations for **True**. Especifica o UPN do proprietário da caixa de correio de destino.|
|DestMailboxOwnerSid|Edm.String|Não|Definido somente se o parâmetro CrossMailboxOperations for **True**. Especifica o SID da caixa de correio de destino.|
|DestMailboxOwnerMasterAccountSid|Edm.String|Não|Definido somente se o parâmetro CrossMailboxOperations for **True**. Especifica o SID do SID da conta principal do proprietário da caixa de correio de destino.|
|DestFolder|Self.[ExchangeFolder](#exchangefolder-complex-type)|Não|A pasta de destino, para operações como Mover.|
|Folders|Collection(Self.[ExchangeFolder](#exchangefolder-complex-type))|Não|Informações sobre as pastas de origem envolvidas em uma operação; por exemplo, se as pastas forem selecionadas e excluídas.|
|AffectedItems|Collection(Self.[ExchangeItem](#exchangeitem-complex-type))|Não|Informações sobre cada item no grupo.|
|||||

### <a name="exchangemailboxauditrecord-schema"></a>Esquema ExchangeMailboxAuditRecord

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Item|Self.[ExchangeItem](#exchangeitem-complex-type)|Não|Representa o item no qual a operação foi executada|
|ModifiedProperties|Collection(Edm.String)|Não|A definir|
|SendAsUserSmtp|Edm.String|Não|Endereço SMTP do usuário que está sendo representado.|
|SendAsUserMailboxGuid|Edm.Guid|Não|O GUID do Exchange da caixa de correio que foi acessada para enviar email.|
|SendOnBehalfOfUserSmtp|Edm.String|Não|O endereço SMTP do usuário em nome de quem o email é enviado.|
|SendOnBehalfOfUserMailboxGuid|Edm.Guid|Não|O GUID do Exchange da caixa de correio que foi acessada para enviar emails.|
|||||

### <a name="exchangeitem-complex-type"></a>Tipo complexo ExchangeItem

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Sim|A ID do repositório.|
|Subject|Edm.String|Não|A linha de assunto da mensagem que foi acessada.|
|ParentFolder|Edm.ExchangeFolder|Não|O nome da pasta onde o item está localizado.|
|Attachments|Edm.String|Não|Uma lista dos nomes e tamanho de arquivo de todos os itens anexados à mensagem.|
|||||

### <a name="exchangefolder-complex-type"></a>Tipo complexo ExchangeFolder

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Sim|A ID do repositório do objeto da pasta.|
|Path|Edm.String|Não|O nome da pasta da caixa de correio em que a mensagem que foi acessada está localizada.|
|||||

## <a name="azure-active-directory-base-schema"></a>Esquema Base do Azure Active Directory

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AzureActiveDirectoryEventType|Self.[AzureActiveDirectoryEventType](#azureactivedirectoryeventtype)|Sim|O tipo de evento do Azure AD. |
|ExtendedProperties|Collection(Common.NameValuePair)|Não|As propriedades estendidas do evento do Azure AD.|
|ModifiedProperties|Collection(Common.ModifiedProperty)|Não|Esta propriedade está incluída para eventos administrativos. A propriedade inclui o nome da propriedade que foi modificada, o novo valor da propriedade modificada e o valor anterior da propriedade modificada.|
|||||

### <a name="enum-azureactivedirectoryeventtype---type--edmint32"></a>Enumeração: AzureActiveDirectoryEventType - Tipo: Edm.Int32

#### <a name="azureactivedirectoryeventtype"></a>AzureActiveDirectoryEventType

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|AccountLogon|O evento de logon da conta.|
|AzureApplicationAuditEvent|O evento de segurança do aplicativo do Azure.|
|||||

## <a name="azure-active-directory-account-logon-schema"></a>Esquema de Logon da Conta do Azure Active Directory

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Application|Edm.String|Não|O aplicativo que dispara o evento de login da conta, como o Office 15.|
|Client|Edm.String|Não|Detalhes sobre o dispositivo cliente, o SO do dispositivo e o navegador do dispositivo que foi usado para o evento de logon da conta.|
|LoginStatus|Edm.Int32|Sim|Esta propriedade é diretamente do OrgIdLogon.LoginStatus. O mapeamento de várias falhas interessantes de logon pode ser feito por meio do alerta de algoritmos.|
|UserDomain|Edm.String|Sim|Informações de Identidade do Locatário (TII, Tenant Identity Information).|
|||||

### <a name="enum-credentialtype---type-edmint32"></a>Enumeração: CredentialType - Tipo: Edm.Int32

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|-1|Other|Outra autenticação.|
|0|Password|A credencial do usuário é o nome de usuário e a senha.|
|1|MobilePhone|A credencial do usuários é o telefone celular.|
|2|SecretQuestion|A credencial do usuário é a pergunta secreta.|
|3|SecurePin|A credencial do usuário é um PIN seguro.|
|4 |SecurePinReset|A credencial do usuário é a redefinição do PIN seguro.|
|11|EasyID|A credencial do usuário é EasyID.|
|14 |PasswordIndexCredentialType|A credencial do usuário é PasswordIndexCredentialType.|
|16|Device|A credencial do usuário é um dispositivo.|
|17 |ForeignRealmIndex|A credencial do usuário é ForeignRealmIndex.|
|||||

### <a name="enum-logintype---type-edmint32"></a>Enumeração: LoginType - Tipo: Edm.Int32

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|-1|Other|Outro tipo de i.|
|1|InitialAuth|Login com autenticação inicial.|
|2|CookieCopy|Login com cookies.|
|3|SilentReAuth|Logon com reautenticação silenciosa.|
|||||

### <a name="enum-authenticationmethod---type-edmint32"></a>Enumeração: AuthenticationMethod - Tipo: Edm.Int32

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Min|O método de autenticação é um Min.|
|1|Password|O método de autenticação é uma senha.|
|2|Digest|O método de autenticação é um resumo.|
|3|ProxyAuth|O método de autenticação é um ProxyAuth.|
|4 |InfoCard|O método de autenticação é um InfoCard.|
|5|DAToken|O método de autenticação é um DAToken.|
|6 |Sha1RememberMyPassword|O método de autenticação é um Sha1RememberMyPassword.|
|7 |LMPasswordHash|O método de autenticação é um LMPasswordHash.|
|8 |ADFSFederatedToken|O método de autenticação é um ADFSFederatedToken.|
|9 |EID|O método de autenticação é um EID.|
|10 |DeviceID|O método de autenticação é um DeviceID. |
|11|MD5|O método de autenticação é MD5.|
|12 |EncProxyPasswordHash|O método de autenticação é um EncProxyPasswordHash.|
|13|LWAFederation|O método de autenticação é um LWAFederation.|
|14 |Sha1HashedPassword|O método de autenticação é um Sha1HashedPassword.|
|15 |SecurePin|O método de autenticação é um Pin seguro.|
|16|SecurePinReset|O método de autenticação é uma redefinição de um Pin seguro.|
|17 |SAML20PostSimpleSign|O método de autenticação é um SAML20PostSimpleSign.|
|18 |SAML20Post|O método de autenticação é um SAML20Post.|
|19|OneTimeCode|O método de autenticação é um código único.|
|||||

## <a name="azure-active-directory-schema"></a>Esquema do Azure Active Directory

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Actor|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|Não|O usuário ou a entidade de serviço que executou a ação.|
|ActorContextId|Edm.String|Não|O GUID da organização à qual o ator pertence.|
|ActorIpAddress|Edm.String|Não|O endereço IP do ator no formato de endereço IPV4 ou IPV6.|
|InterSystemsId|Edm.String|Não|O GUID que controla as ações entre componentes dentro do serviço do Office 365.|
|IntraSystemsId|Edm.String|Não|O GUID gerado pelo Azure Active Directory para acompanhar a ação.|
|SupportTicketId|Edm.String|Não|O ID do ticket de suporte ao cliente para a ação em situações de "agir em nome de".|
|Target|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|Não|O usuário em que a ação (identificada pela propriedade Operation) foi executada.|
|TargetContextId|Edm.String|Não|O GUID da organização à qual o usuário de destino pertence.|
|||||

### <a name="complex-type-identitytypevaluepair"></a>Tipo complexo IdentityTypeValuePair

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|ID|Edm.String|Sim|O valor da identidade de acordo com o tipo.|
|Type|Self.IdentityType|Sim|O tipo da identidade.|
|||||

### <a name="enum-identitytype---type-edmint32"></a>Enumeração: IdentityType - Tipo: Edm.Int32

#### <a name="identitytype"></a>IdentityType

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|Claim|A identidade é uma declaração para fins de autorização.|
|Name|O ator de ação de auditoria ou o nome de exibição da identidade de destino.|
|Other|A identidade do ator é outro tipo, como o ObjectId no GUID gerado pelo serviço do Office 365.|
|PUID|O ator da ação de auditoria ou a ID exclusiva do passaporte de destino (PUID).|
|SPN|A identidade de uma entidade de segurança de serviço, se a ação for executada pelo serviço do Office 365.|
|UPN|O nome da entidade de segurança do usuário.|
|||||

## <a name="azure-active-directory-secure-token-service-sts-logon-schema"></a>Esquema de logon do Serviço de Token Seguro (STS) do Active Directory do Azure

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|Não|O GUID que representa o aplicativo que está solicitando o logon. O nome de exibição pode ser pesquisado por meio da API de gráfico do Azure Active Directory.|
|Client|Edm.String|Não|Informações do dispositivo cliente, fornecidas pelo navegador que executa o logon.|
|DeviceProperties|Collection(Common.NameValuePair)|Não|Esta propriedade inclui vários detalhes do dispositivo, incluindo Id, Nome do Display, OS, Browser, IsCompliant, IsCompliantAndManaged, SessionId e DeviceTrustType. A propriedade DeviceTrustType pode ter os seguintes valores:<br/><br/>**0** - Registrado no Microsoft Azure AD<br/> **1** - Ingressado no Microsoft Azure AD<br/> **2** - Ingressado no Azure AD Híbrido|
|ErrorCode|Edm.String|Não|Para logons com falha (em que o valor da propriedade Operation é UserLoginFailed), essa propriedade contém o código de erro STS do Azure Active Directory (AADSTS). Para obter descrições desses códigos de erro, confira [Códigos de erro de autenticação e autorização](/azure/active-directory/develop/reference-aadsts-error-codes#aadsts-error-codes). Um valor de `0` indica um logon bem-sucedido.|
|LogonError|Edm.String|Não|Para logons com falha, esta propriedade contém uma descrição legível pelo usuário do motivo do logon com falha.|
|||||

## <a name="dlp-schema"></a>Esquema DLP

Os eventos de DLP estão disponíveis para o Exchange Online, o SharePoint Online e o OneDrive For Business. Observe que os eventos de DLP no Exchange só estão disponíveis para eventos com base na política DLP unificada (por exemplo, configurados por meio do Centro de Conformidade e Segurança). Não há suporte para eventos de DLP com base em Regras de Transporte do Exchange.

Os eventos de DLP (Prevenção contra Perda de Dados) sempre terão UserKey = "DlpAgent" no esquema Comum. Existem três tipos DlpEvents que estão armazenados como o valor da propriedade Operation do esquema comum:

- DlpRuleMatch – indica que uma regra foi correspondida. Esses eventos existem no Exchange, no SharePoint Online e no OneDrive for Business. Para o Exchange, inclui informações de falsos positivos e de substituição. Para o SharePoint Online e o OneDrive for Business, falsos positivos e as substituições geram eventos separados.

- DlpRuleUndo – existem apenas no SharePoint Online e no OneDrive for Business e indicam que uma ação de política aplicada anteriormente foi "desfeita" – seja por causa de designação de falso positivo/substituição pelo usuário, ou porque o documento não está mais sujeito à política (seja devido a mudança de política ou alteração de conteúdo no documento).

- DlpInfo – existem apenas no SharePoint Online e no OneDrive for Business e indicam uma designação de falso positivo, mas nenhuma ação foi "desfeita".

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.[SharePointMetadata](#sharepointmetadata-complex-type)|Não|Descreve os metadados sobre o documento no SharePoint ou o OneDrive for Business que continham as informações confidenciais.|
|ExchangeMetaData|Self.[ExchangeMetadata](#exchangemetadata-complex-type)|Não|Descreve os metadados sobre a mensagem de email que continha as informações confidenciais.|
|ExceptionInfo|Edm.String|Não|Identifica os motivos pelos quais uma política não se aplica mais e/ou qualquer informação sobre falso positivos e/ou substituição observada pelo usuário final.|
|PolicyDetails|Collection(Self.[PolicyDetails](#policydetails-complex-type))|Sim|Informações sobre uma ou mais políticas que dispararam o evento de DLP.|
|SensitiveInfoDetectionIsIncluded|Boolean|Sim|Indica se o evento contém o valor do tipo de dados confidenciais e o contexto adjacente do conteúdo de origem. O acesso a dados confidenciais exige a permissão "Ler eventos de política DLP, incluindo detalhes confidenciais" no Azure Active Directory.|
|||||

### <a name="sharepointmetadata-complex-type"></a>Tipo complexo SharePointMetadata

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|From|Edm.String|Sim|O usuário que disparou o evento. Será FileOwner, LastModifier ou LastSharer.|
|itemCreationTime|Edm.Date|Sim|Datetimestamp em UTC de quando o evento foi registrado.|
|SiteCollectionGuid|Edm.Guid|Sim|O GUID do conjunto de sites.|
|SiteCollectionUrl|Edm.String|Sim|Nome do site do SharePoint.|
|FileName|Edm.String|Sim|Nome do caminho.|
|FileOwner|Edm.String|Sim|O proprietário do documento.|
|FilePathUrl|Edm.String|Sim|A URL do documento.|
|DocumentLastModifier|Edm.String|Sim|O usuário que modificou o documento pela última vez.|
|DocumentSharer|Edm.String|Sim|O usuário que modificou pela última vez o compartilhamento do documento.|
|UniqueId|Edm.String|Sim|Uma GUID que identifica o arquivo.|
|LastModifiedTime|Edm.DateTime|Sim|Carimbo de data/hora em UTC de quando o documento foi modificado pela última vez.|
|IsViewableByExternalUsers|Edm.Boolean|Sim|Determina se o arquivo é acessível para qualquer usuário externo.|
|||||

### <a name="exchangemetadata-complex-type"></a>Tipo complexo ExchangeMetadata

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|MessageID|Edm.String|Sim|A ID da mensagem do email que disparou o evento.|
|From|Edm.String|Sim|O usuário que enviou o email.|
|To|Collection(Edm.String)|Não|Uma coleção de endereços de email que estavam na linha Para da mensagem.|
|CC|Collection(Edm.String)|Não|Uma coleção de endereços de email que estavam na linha CC da mensagem.|
|BCC|Collection(Edm.String)|Não|Uma coleção de endereços de email que estavam na linha BCC da mensagem.|
|Subject|Edm.String|Sim|Assunto da mensagem de email.|
|Sent|Edm.DateTime|Sim|O horário em UTC de quando o email foi enviado.|
|RecipientCount|Edm.Int32|Sim|O número total de todos os destinatários nas linhas TO, CC e BCC da mensagem.|
|||||

### <a name="policydetails-complex-type"></a>Tipo complexo PolicyDetails

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|PolicyId|Edm.Guid|Sim|O GUID da política DLP para este evento.|
|PolicyName|Edm.String|Sim|O nome amigável da política DLP para este evento.|
|Rules|Collection(Self.[Rules](#rules-complex-type))|Sim|As informações sobre as regras da política que foram atendidas para este evento.|
|||||

### <a name="rules-complex-type"></a>Tipo complexo Rules

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|RuleId|Edm.Guid|Sim|O GUID da regra DLP para este evento.|
|RuleName|Edm.String|Sim|O nome amigável da regra DLP para este evento.|
|Actions|Collection(Edm.String)|Não|Uma lista de ações tomadas como resultado de um evento DLP RuleMatch.|
|OverriddenActions|Collection(Edm.String)|Não|Uma lista de ações realizadas anteriormente que foram desfeitas como resultado de um evento DLPRuleUndo. |
|Severity|Edm.String|Não|A gravidade (Baixa, Média e Alta) da conformidade com a regra.|
|RuleMode|Edm.String|Sim|Indique se a regra DLP foi definida como Enforce, Audit with Notify ou somente Audit.|
|ConditionsMatched|Self.[ConditionsMatched](#conditionsmatched-complex-type)|Não|Os detalhes sobre quais condições da regra foram atendidas para este evento.|
|||||

### <a name="conditionsmatched-complex-type"></a>Tipo complexo ConditionsMatched

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.[SensitiveInformation](#sensitiveinformation-complex-type))|Não|As informações sobre o tipo de informações confidenciais detectadas.|
|DocumentProperties|Collection(NameValuePair)|Não|As informações sobre as propriedades do documento que dispararam uma correspondência de regra.|
|OtherConditions|Collection(NameValuePair)|Não|Uma lista de pares de valores chave que descrevem quaisquer outras condições que foram correspondidas.|
|||||

### <a name="sensitiveinformation-complex-type"></a>Tipo complexo SensitiveInformation

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.Int|Sim|A confiança agregada de todos os padrões corresponde ao Tipo de Informação Confidencial.|
|Count|Edm.Int|Sim|O número total de instâncias confidenciais detectadas.|
|Local|Edm.String|Não||
|SensitiveType|Edm.Guid|Sim|Um guid que identifica o tipo de dados confidenciais detectados.|
|SensitiveInformationDetections|Self.SensitiveInformationDetections|Não|Uma matriz de objetos que contêm dados de informações confidenciais com os seguintes detalhes – valor correspondente e contexto do valor correspondente.|
|SensitiveInformationDetailedClassificationAttributes|Coleção(SensitiveInformationDetailedConfidenceLevelResult)|Sim|Informações sobre a contagem do tipo de informação confidencial detectada para cada um dos três níveis de confiança (Alto, Médio e Baixo) e se ela corresponde à regra DLP ou não|
|SensitiveInformationTypeName|Edm.String|Não|O nome do tipo de informação confidencial.|
|UniqueCount|Edm.Int32|Sim|A contagem exclusiva de instâncias confidenciais detectadas.|
|||||

### <a name="sensitiveinformationdetailedclassificationattributes-complex-type"></a>Tipo complexo SensitiveInformationDetailedClassificationAttributes

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.int32|Sim|O nível de confiança do padrão que foi detectado.|
|Contar|Edm.Int32|Sim|O número de instâncias confidenciais detectadas para um nível de particular de confiança.|
|IsMatch|Edm.Boolean|Sim|Indica se a contagem fornecida e o nível de confiança do tipo confidencial detectado resulta em uma correspondência de regra de DLP.|
|||||

### <a name="sensitiveinformationdetections-complex-type"></a>Tipo complexo SensitiveInformationDetections

Os dados confidenciais de DLP só estão disponíveis na API do feed de atividades para usuários que receberam permissões para "Ler dados confidenciais de DLP".

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|ValoresDetectados|Coleção(Common.NameValuePair)|Sim|Uma matriz de informações confidenciais que foram detectadas. As informações contêm pares de valores chave com Valor = valor correspondente (por exemplo, Valor do cartão de crédito) e Contexto = um trecho do conteúdo da origem que contém o valor correspondente. |
|ResultsTruncated|Edm.Boolean|Sim|Indica se os logs foram truncados devido ao grande número de resultados. |
|||||

### <a name="exceptioninfo-complex-type"></a>Tipo complexo ExceptionInfo

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Reason|Edm.String|Não|Para um evento DLPRuleUndo, isso indica por que a regra não se aplica mais, o que pode ocorrer devido a um dos três motivos: substituição, alteração de documento ou alteração de política|
|FalsePositive|Edm.Boolean|Não|Indica se o usuário designou esse evento como falso positivo.|
|Justification|Edm.String|Não|Se o usuário tiver optado por substituir a política, qualquer justificativa especificada pelo usuário será capturada aqui.|
|Rules|Collection(Edm.Guid)|Não|Uma coleção de guids para cada regra designada como falso positivo ou substituta, ou para a qual uma ação foi desfeita.|
|||||

## <a name="security-and-compliance-center-schema"></a>Esquema do Centro de Conformidade e Segurança

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|Não|A data e hora em que o cmdlet foi executado.|
|ClientRequestId|Edm.String|Não|Um GUID que pode ser usado para correlacionar este cmdlet com as operações UX do Centro de Conformidade e Segurança. Essas informações são usadas apenas pelo suporte da Microsoft.|
|CmdletVersion|Edm.String|Não|A versão de build do cmdlet quando foi executado.|
|EffectiveOrganization|Edm.String|Não|O GUID da organização afetada pelo cmdlet. (Descontinuado: este parâmetro será retirado no futuro.)|
|UserServicePlan|Edm.String|Não|O plano de serviço Proteção do Exchange Online atribuído ao usuário que executou o cmdlet.|
|ClientApplication|Edm.String|Não|Se o cmdlet tiver sido executado por um aplicativo, ao contrário do PowerShell remoto, esse campo conterá o nome desse aplicativo.|
|Parâmetros|Edm.String|Não|O nome e o valor dos parâmetros que foram usados ​​com o cmdlet que não incluem Informações de Identificação Pessoal.|
|NonPiiParameters|Edm.String|Não|O nome e o valor dos parâmetros que foram usados ​​com o cmdlet que incluem Informações de Identificação Pessoal. (Preterido: esse campo será retirado no futuro e seu conteúdo mesclado com o campo de parâmetros.)|
|||||

## <a name="security-and-compliance-alerts-schema"></a>Esquema de Alertas de Conformidade e Segurança

Os sinais de alerta incluem:

- Todos os alertas gerados baseados nas [Políticas de alerta no Centro de Conformidade e Segurança](/office365/securitycompliance/alert-policies#default-alert-policies).
- Alertas relacionados ao Office 365 gerados no [Office 365 Cloud App Security](/office365/securitycompliance/office-365-cas-overview) e no [Microsoft Cloud App Security](/cloud-app-security/what-is-cloud-app-security).

O UserId e o UserKey desses eventos são sempre SecurityComplianceAlerts. Existem três tipos de alertas de eventos que estão armazenados como o valor da propriedade Operation do esquema comum:

- AlertTriggered – um novo alerta é gerado devido a uma correspondência com a política.

- AlertEntityGenerated – uma nova entidade é adicionada a um alerta. Este evento somente é aplicável aos alertas gerados com base nas Políticas de Alerta no Centro de Conformidade e Segurança. Cada alerta gerado pode ser associado a um ou vários desses eventos. Por exemplo, uma política de alerta é definida para disparar um alerta se um usuário exclui mais de 100 arquivos em cinco minutos. Se dois usuários ultrapassarem o limite ao mesmo tempo, haverá dois eventos AlertEntityGenerated, mas apenas um evento AlertTriggered.

- AlertUpdated - uma atualização foi feita aos metadados de um alerta. Esse evento é registrado quando o status de um alerta é alterado (por exemplo, de "ativo" para "resolvido") e quando alguém adiciona um comentário ao alerta.

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AlertId|Edm.Guid|Sim|O GUID do alerta.|
|AlertType|Self.String|Sim|Tipo do alerta. Os tipos de alertas incluem: <ul><li>Sistema</li><li>Personalizado|
|Name|Edm.String|Sim|Nome do alerta.|
|PolicyId|Edm.Guid|Não|O GUID da política que disparou o alerta.|
|Status|Edm.String|Não|Status do alerta. Os status incluem: <ul><li><p>Ativo</li><li>Investigando</li><li>Resolvido</li><li>Descartado|
|Severity|Edm.String|Não|Gravidade do alerta. Os níveis de gravidade incluem: <ul><li>Baixo</li><li>Médio</li><li>Alto</li></ul>|
|Categoria|Edm.String|Não|Categoria do alerta. As categorias incluem: <ul><li>AccessGovernance</li><li>DataGovernance</li><li>DataLossPrevention</li><li>InsiderRiskManagement</li><li>MailFlow</li><li>ThreatManagement</li><li>Outros|
|Source|Edm.String|Não|Fonte do alerta. As fontes incluem: <ul><li>Conformidade e Segurança do Office 365</li><li>Segurança no Aplicativo na Nuvem|
|Comments|Edm.String|Não|Comentários realizados pelos usuários que visualizaram o alerta. Por padrão, é "Novo alerta".|
|Data|Edm.String|Não|O BLOB de dados de detalhes do alerta ou da entidade de alerta.|
|AlertEntityId|Edm.String|Não|O identificador da entidade de alerta. Esse parâmetro só é aplicável em eventos AlertEntityGenerated.|
|EntityType|Edm.String|Não|Tipo de alerta ou entidade de alerta. Os tipos de entidade incluem: <ul><li>User</li><li>Recipients</li><li>Sender</li><li>MalwareFamily</li></ul>Esse parâmetro só é aplicável em eventos AlertEntityGenerated.|
|||||

## <a name="yammer-schema"></a>Esquema do Yammer

Os eventos do Yammer listados em [Pesquisar no log de auditoria no Centro de Conformidade e Segurança](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#yammer-activities) usarão esse esquema.

|**Parâmetros**|**Tipo**|**Obrigatório**|**Descrição**|
|:-----|:-----|:-----|:-----|
|ActorUserId|Edm.String|Não|Email do usuário que executou a operação.|
|ActorYammerUserId|Edm.Int64|Não|ID do usuário que executou a operação.|
|DataExportType|Edm.String|Não|Retorna "dados" se a exportação de dados incluir mensagens, notas, arquivos, tópicos, usuários e grupos; retorna "usuário" se a exportação de dados incluir apenas usuários.|
|FileId|Edm.Int64|Não|ID do arquivo na operação. |
|FileName|Edm.String|Não|Nome do arquivo na operação. Será exibido em branco se não for relevante para a operação.|
|GroupName|Edm.String|Não|Nome do grupo na operação. Será exibido em branco se não for relevante para a operação.|
|IsSoftDelete|Edm.Boolean|Não|Retorna "true" se a política de retenção de dados da rede estiver definida como exclusão reversível; retorna "false" se a política de retenção de dados da rede estiver definida como Exclusão Irreversível.|
|MessageId|Edm.Int64|Não|ID da mensagem na operação.|
|YammerNetworkId|Edm.Int64|Não|ID da rede do usuário que executou a operação.|
|TargetUserId|Edm.String|Não|Email do usuário de destino na operação. Será exibido em branco se não for relevante para a operação.|
|TargetYammerUserId|Edm.Int64|Não|ID do usuário de destino na operação.|
|VersionId|Edm.Int64|Não|ID da versão do arquivo na operação.|
|||||

## <a name="data-center-security-base-schema"></a>Esquema Base de Segurança do Data Center

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.[DataCenterSecurityEventType](#datacentersecurityeventtype)|Sim|O tipo de evento cmdlet na caixa de bloqueio.|
|||||

### <a name="enum-datacentersecurityeventtype---type-edmint32"></a>Enumeração: DataCenterSecurityEventType: - Tipo: Edm.Int32

#### <a name="datacentersecurityeventtype"></a>DataCenterSecurityEventType

|**Nome do membro**|**Descrição**|
|:-----|:-----|
|DataCenterSecurityCmdletAuditEvent|Esse é o valor de enumeração para o evento de tipo de auditoria de cmdlet.|
|||

## <a name="data-center-security-cmdlet-schema"></a>Esquema Cmdlet de Segurança do Data Center

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|Sim|O horário de início da execução do cmdlet.|
|EffectiveOrganization|Edm.String|Sim|O nome do locatário para o qual a elevação/cmdlet foi direcionado.|
|ElevationTime|Edm.Date|Sim|O horário de início da elevação.|
|ElevationApprover|Edm.String|Sim|O nome de um gerente da Microsoft.|
|ElevationApprovedTime|Edm.Date|Não|O carimbo de data/hora para quando a elevação foi aprovada.|
|ElevationRequestId|Edm.Guid|Sim|Um identificador exclusivo para a solicitação de elevação.|
|ElevationRole|Edm.String|Não|A função da elevação.|
|ElevationDuration|Edm.Int32|Sim|A duração para a qual a elevação estava ativa.|
|GenericInfo|Edm.String|Não|Usada para comentários e outras informações genéricas.|
|||||

## <a name="microsoft-teams-schema"></a>Esquema do Microsoft Teams

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AddOnGuid|Edm.Guid|Não|O identificador exclusivo do complemento que gerou esse evento.|
|AddOnName|Edm.String|Não|O nome do complemento que gerou esse evento.|
|AddOnType|Self.[AddOnType](#addontype)|Não|O tipo de complemento que gerou esse evento.|
|ChannelGuid|Edm.Guid|Não|Um identificador exclusivo para o canal que está sendo auditado.|
|ChannelName|Edm.String|Não|O nome do canal que está sendo auditado.|
|ChannelType|Edm.String|Não|O tipo de canal que está sendo auditado (padrão/particular).|
|ExtraProperties|Coleção (Self.[ KeyValuePair](#keyvaluepair-complex-type))|Não|Uma lista de propriedades adicionais.|
|HostedContents|Coleção (Self.[HostedContent](#hostedcontent-complex-type))|Não|Uma coleção de conteúdo hospedado por mensagem de chat ou canal.|
|Members|Collection(Self.[MicrosoftTeamsMember](#microsoftteamsmember-complex-type))|Não|Uma lista de usuários dentro de uma equipe.|
|MessageId|Edm.String|Não|Um identificador para uma mensagem de bate-papo ou canal.|
|MessageURLs|Edm.String|Não|Presente para qualquer URL enviado nas mensagens do Teams.|
|Mensagens|Coleção (Self.[Message](#message-complex-type))|Não|Uma coleção de mensagens de chat ou canal.|
|MessageSizeInBytes|Edm.Int64|Não|O tamanho de uma mensagem de chat ou canal em bytes com codificação UTF-16.|
|Nome|Edm.String|Não|Só apresenta para eventos de configurações. Nome da configuração que foi alterada.|
|NewValue|Edm.String|Não|Só apresenta para eventos de configurações. Novo valor da configuração.|
|OldValue|Edm.String|Não|Só apresenta para eventos de configurações. Valor antigo da configuração.|
|SubscriptionId|Edm.String|Não|Um identificador exclusivo de uma assinatura de notificação de alteração do Microsoft Graph.|
|TabType|Edm.String|Não|Só apresenta para eventos de tabulação. O tipo de guia que gerou esse evento.|
|TeamGuid|Edm.Guid|Não|Um identificador exclusivo para a equipe que está sendo auditada.|
|TeamName|Edm.String|Não|O nome da equipe que está sendo auditada.|
||||

### <a name="microsoftteamsmember-complex-type"></a>Tipo complexo MicrosoftTeamsMember

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|UPN|Edm.String|Não|O nome da entidade de segurança do usuário.|
|Role|Self.[MemberRoleType](#memberroletype)|Não|A função de usuário dentro da equipe.|
|DisplayName|Edm.String|Não|O nome de exibição do usuário.|
|||||

### <a name="enum-memberroletype---type-edmint32"></a>Enumeração: MemberRoleType - Tipo: Edm.Int32

#### <a name="memberroletype"></a>MemberRoleType

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Member|Um usuário que é um membro da equipe.|
|1|Owner|Um usuário que é o proprietário da equipe.|
|2|Guest|Um usuário que não é um membro da equipe.|
||||

### <a name="keyvaluepair-complex-type"></a>KeyValuePair tipo complexo

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Chave|Edm.String|Não|A chave do par de valor-chave.|
|Valor|Edm.String|Não|O valor do par de valor-chave.|
|||||

### <a name="enum-addontype---type-edmint32"></a>Enumeração: AddOnType - Tipo: Edm.Int32

#### <a name="addontype"></a>AddOnType

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|1|Bot|Um bot do Microsoft Teams.|
|2|Connector|Um conector do Microsoft Teams.|
|3|Tab|Uma guia do Microsoft Teams.|
||||

### <a name="hostedcontent-complex-type"></a>Tipo complexo HostedContent

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|Sim|Um identificador exclusivo do conteúdo hospedado da mensagem.|
|Sizeinbytes|Edm.Int64|Não|O tamanho do conteúdo hospedado da mensagem em bytes.|
|||||

### <a name="message-complex-type"></a>Tipo complexo de mensagem

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AADGroupId|Edm.String|Não|Um identificador exclusivo do grupo no Azure Active Directory ao qual a mensagem pertence.|
|Id|Edm.String|Sim|Um identificador exclusivo da mensagem de chat ou canal.|
|ChannelGuid|Edm.String|Não|Um identificador exclusivo do canal ao qual a mensagem pertence.|
|ChannelName|Edm.String|Não|O nome do canal ao qual a mensagem pertence.|
|ChannelType|Edm.String|Não|O tipo do canal ao qual a mensagem pertence.|
|Nome do Chat|Edm.String|Não|O nome do chat ao qual a mensagem pertence.|
|ChatThreadId|Edm.String|Não|Um identificador exclusivo do chat ao qual a mensagem pertence.|
|ParentMessageId|Edm.String|Não|Um identificador exclusivo da mensagem do canal ou chat pai.|
|Sizeinbytes|Edm.Int64|Não|O tamanho da mensagem em bytes com codificação UTF-16.|
|TeamGuid|Edm.String|Não|Um identificador exclusivo da equipe à qual a mensagem pertence.|
|TeamName|Edm.String|Não|O nome da equipe à qual a mensagem pertence.|
|Versão|Edm.String|Não|A versão do chat ou mensagem do canal.|
|||||
 
## <a name="microsoft-defender-for-office-365-and-threat-investigation-and-response-schema"></a>Microsoft Defender para Office 365 e esquema de investigação e resposta de ameaças

[Microsoft Defender para Office 365](/office365/securitycompliance/office-365-atp) e eventos de Investigação e Resposta a Ameaças estão disponíveis para clientes do Office 365 que tenham um Plano 1 do Defender para Office 365, Plano 2 do Defender para Office 365 ou uma assinatura E5.  Cada evento no feed do Defender para Office 365 corresponde aos seguintes eventos que foram determinados como contendo uma ameaça:

- Detecção em mensagens de email enviadas ou recebidas por um usuário na organização no momento da entrega e na [Limpeza automática zero hora](https://support.office.com/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15). 

- URLs clicados por um usuário na organização que foram detectados como maliciosos no momento do clique com base na proteção [Links Seguros no Defender para Office 365](/office365/securitycompliance/atp-safe-links).  

- Um arquivo no SharePoint Online, OneDrive for Business ou Microsoft Teams que foi detectado como malicioso pela proteção do [Microsoft Defender para Office 365](/office365/securitycompliance/atp-for-spo-odb-and-teams).

- Um alerta que é acionado e inicia uma [investigação automática](/office365/securitycompliance/automated-investigation-response-office).

> [!NOTE]
> As capacidades do Microsoft Defender para Office 365 e da Investigação e Resposta a Ameaças do Office 365(anteriormente conhecidos como Inteligência contra Ameaças do Office 365) agora fazem parte do Plano 2 do Defender para Office 365, com recursos adicionais de proteção contra ameaças.  Para saber mais, consulte [Planos e preços do Microsoft Defender para Office 365](https://products.office.com/exchange/advance-threat-protection) e a [Descrição do serviço Defender para Office 365](/office365/servicedescriptions/office-365-advanced-threat-protection-service-description).

### <a name="email-message-events"></a>Eventos de mensagens de email

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AttachmentData|Collection(Self.[AttachmentData](#attachmentdata))|Não|Os dados sobre anexos na mensagem de email que acionaram o evento.|
|Tipo de detecção|Edm.String|Sim|O tipo de detecção (por exemplo, **Embutido** – detectada na hora da entrega; **Atrasado** – detectada após a entrega; **ZAP** – mensagens removidas pela [Limpeza Automática Zero Hora](https://support.office.com/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15)). Os eventos com o tipo de detecção ZAP normalmente são precedidos por uma mensagem com um tipo de detecção **Atrasado**.|
|DetectionMethod|Edm.String|Sim|O método ou tecnologia usada pelo Defender for Office 365 para a detecção.|
|InternetMessageId|Edm.String|Sim|A ID da mensagem da Internet.|
|NetworkMessageId|Edm.String|Sim|A ID da mensagem de rede do Exchange Online.|
|P1Sender|Edm.String|Sim|O caminho de retorno do remetente da mensagem de email.|
|P2Sender|Edm.String|Sim|O remetente da mensagem de email.|
|Política|Self.[Policy](#policy-type-and-action-type)|Sim|O tipo de política de filtragem (por exemplo, **Anti-spam** ou **Anti-phishing**) e o tipo de ação relacionada (por exemplo, **Spam de Alta Confiança**, **Spam** ou **Phishing**) relevante para a mensagem de email.|
|Política|Self.[PolicyAction](#policy-action)|Sim|A ação configurada na política de filtragem (por exemplo, **Mover para a pasta Lixo Eletrônico** ou **Quarentena**) relevante para a mensagem de email.|
|P2Sender|Edm.String|Sim|O **Remetente:** da mensagem de email.|
|Destinatários|Collection(Edm.String)|Sim|Uma matriz de destinatários da mensagem de email.|
|SenderIp|Edm.String|Sim|O endereço IP que enviou o email do Office 365. O endereço IP é exibido em um formato de endereço IPv4 ou IPv6.|
|Subject|Edm.String|Sim|A linha de assunto da mensagem.|
|Verdict|Edm.String|Sim|A conclusão da mensagem.|
|MessageTime|Edm.Date|Sim|Data e hora em UTC (Tempo Universal Coordenado), na qual a mensagem de email foi recebida ou enviada.|
|EventDeepLink|Edm.String|Sim|Link profundo para o evento de email no Explorador ou para relatórios em tempo real no Centro de Conformidade e Segurança do Office 365.|
|Ação de Entrega |Edm.String|Sim|A ação de entrega original na mensagem.|
|Local de Entrega original |Edm.String|Sim|O local de entrega original da mensagem.|
|Local de Entrega mais recente |Edm.String|Sim|O local de entrega mais recente da mensagem no momento do evento.|
|Directionality |Edm.String|Sim|Identifica se uma mensagem foi de entrada, de saída ou de dentro da organização.|
|ThreatsAndDetectionTech |Edm.String|Sim|As ameaças e as tecnologias de detecção correspondentes. Este campo expõe todas as ameaças em uma mensagem, incluindo a adição mais recente no veredicto de spam.  Por exemplo, ["Phishing: [falsificar DMARC]", "Spam: [URL de reputação maliciosa]"]. As diferentes ameaças de detecção e tecnologias de detecção são descritas a seguir.|
|AdditionalActionsAndResults |Collection(Edm.String)|Não|As ações adicionais realizadas no email, como ZAP ou Correção Manual. Também inclui os resultados correspondentes.|
|Conectores |Edm.String|Não|Os nomes e GUIDs dos conectores associados ao email.|
|AuthDetails |Collection(Self.[AuthDetails](#authdetails))|Não|As verificações de autenticação feitas para o email. Também inclui os valores de SPF, DKIM, DMARC e CompAuth.|
|SystemOverrides |Collection(Self.[SystemOverrides](#systemoverrides))|Não|Substituições que são aplicáveis ao email. Estas podem ser anulações do sistema ou do usuário.|
|Nível de Confiança de Phishing |Edm.String|Não|Indica o nível de confiança associado ao veredicto de Phishing. Pode ser Normal ou Alto.|  
|||||

> [!NOTE]
> Recomendamos o uso do novo campo ThreatsAndDetectionTech porque ele mostra vários veredictos e as tecnologias de detecção atualizadas. Isso também se alinha com os valores observados em outras experiências, como no Explorador de ameaças e na busca avançada de ameaças. 

### <a name="detection-technologies"></a>Tecnologias de detecção

|**Nome**|**Descrição**|
|:-----|:-----|
|Filtro geral |Sinais de phishing baseados em regras.|
|Marca de usurpação de identidade | O tipo de arquivo do anexo.|
|Domínio externo falso |O remetente está tentando falsificar algum outro domínio.|
|DMARC falso |Falha de autenticação DMARC para mensagens.|
|Domínio de usurpação de identidade | Usurpação de identidade de domínios que o cliente possui ou define.|
|Detonação de arquivo |Anexos de arquivo considerados perigosos durante a análise de detonação.|
|Reputação de arquivos |Anexos de arquivo marcados como perigosos devido à má reputação.|
|Reputação da detonação de arquivo |Anexo de arquivo marcado como perigoso devido à reputação da detonação anterior.|
|Correspondência de impressão digital |A mensagem foi marcada como perigosa devido a mensagens anteriores.|
|Usurpação de identidade da inteligência de caixa de correio |Usurpação de identidade baseada na inteligência de caixa de correio.|
|Reputação do domínio |Análise baseada na reputação do domínio.|
|Falsificação dentro da organização |  O remetente está tentando falsificar o domínio do destinatário. |
|Filtro avançado |  Sinais de phishing com base no aprendizado de máquina.|
|Mecanismo antimalware    | Detecção de mecanismos antimalware. |
|Detecção de análise mista   | Vários filtros contribuíram para o veredicto desta mensagem. |
|Reputação mal-intencionada de URL   | A mensagem foi considerada perigosa devido a uma URL mal-intencionada. |
|Detonação de URL | A mensagem foi considerada perigosa devido a uma detonação de URL maliciosa anterior. |
|Reputação da detonação de URL| A mensagem foi considerada perigosa devido à detonação de URL mal-intencionada. |
|Usuário de usurpação de identidade|    Usurpação de identidade de usuários definida pelo administrador ou aprendida por meio da inteligência de caixa de correio.|
|Campanha   |Mensagens identificadas como parte de uma campanha.|

### <a name="attachmentdata-complex-type"></a>Tipo complexo AttachmentData

#### <a name="attachmentdata"></a>AttachmentData

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|Sim|O nome do arquivo do anexo.|
|FileType|Edm.String|Sim|O tipo de arquivo do anexo.|
|FileVerdict|Self.[FileVerdict](#fileverdict)|Sim|O veredicto de malware do arquivo.|
|MalwareFamily|Edm.String|Não|A família de malware do arquivo.|
|SHA256|Edm.String|Sim|O hash SHA256 do arquivo.|
|||||

> [!NOTE]
> Dentro da família Malware, você poderá ver o nome exato do MalwareFamily (por exemplo, HTML/Phish.VS! MSR) ou carga maliciosa como uma cadeia de caracteres estática. Uma carga maliciosa ainda deve ser tratada como email malicioso quando um nome específico não é identificado.

### <a name="systemoverrides-complex-type"></a>Tipo complexo SystemOverrides
 
#### <a name="systemoverrides"></a>SystemOverrides

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Detalhes|Edm.String|Não|Os detalhes sobre a substituição específica (como ETR ou Remetente seguro) que foi aplicada.|
|FinalOverride|Edm.String|Não|Indica a substituição que afetou a entrega no caso de várias substituições.|
|Resultado|Edm.String|Não|Indica se o email foi definido como permitido ou bloqueado com base na substituição.|
|Source|Edm.String|Não|Indica se a substituição foi configurada pelo usuário ou pelo locatário.|
|||||

### <a name="authdetails-complex-type"></a>Tipo complexo AuthDetails
 
#### <a name="authdetails"></a>AuthDetails
 
|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Nome|Edm.String|Não|O nome da verificação de autenticação específica, como DKIM ou DMARC.|
|Valor|Edm.String|Não|O valor associado à verificação de autenticação específica, como Verdadeiro ou Falso.|
|||||
 
### <a name="enum-fileverdict---type-edmint32"></a>Enumeração: FileVerdict - Tipo: Edm.Int32

#### <a name="fileverdict"></a>FileVerdict

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Good|Nenhuma ameaça detectada.|
|1|Bad|Malware encontrado no anexo.|
|-1|Error|Erro de varredura/análise.|
|-2|Timeout|Tempo limite de varredura/análise.|
|-3|Pending|Varredura/análise não concluída.|
|||||

### <a name="enum-policy---type-edmint32"></a>Enumeração: Policy - Tipo: Edm.Int32

#### <a name="policy-type-and-action-type"></a>Tipo de política e tipo de ação

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|1|Anti-spam, HSPM|Ação de HSPM (Spam de Alta Confiança) na política anti-spam.|
|2|Anti-spam, SPM|Ação de Spam (SPM) na política anti-spam.|
|3|Anti-spam, em massa|Ação em massa na política anti-spam.|
|4 |Anti-spam, PHSH|Ação de PHSH (phishing) na política anti-spam.|
|5|Anti-phishing, DIMP|Ação de Representação de Domínio (DIMP) na política anti-phishing.|
|6 |Anti-phishing, UIMP|Ação de Representação de Usuários (UIMP) na política anti-phishing.|
|7 |Anti-phishing, SPOOF|Ação falsa na política anti-phishing.|
|8 |Anti-phishing, GIMP|Ação de inteligência da caixa de correio na política Antiphishing.|
|9 |Antimalware, AMP| Ação de política de malware na política antimalware.|
|10 |Anexo seguro, SAP| Ação da política nos anexos Seguros na política do Defender para Office 365.|
|11|Regra de transporte do Exchange, ETR| Ação de diretiva na Regra de Transporte do Exchange.|
|12 |Antimalware, ZAPM| Ação de política de malware na política antimalware aplicada ao ZAP (zero-hour purge).|
|13|Anti-phishing, ZAPP| Ação de política de phishing na política anti-phishing aplicada ao ZAP.|
|14 |Anti-phishing, ZAPS| Ação de política de spam na política antispam aplicada ao ZAP.|
|15 |Antispam, email de phishing de alta confiança (HPHISH)|Ação de política de phishing de alta confiabilidade na política antispam.|
|17 |Antispam, política de spam de saída (OSPM)|Ação de política na política de filtro de spam de saída em Antispam.|
||||

### <a name="enum-policyaction---type-edmint32"></a>Enumeração: PolicyAction - Tipo: Edm.Int32

#### <a name="policy-action"></a>Ação de política

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|MoveToJMF|A ação de política é mover para a pasta Lixo Eletrônico.|
|1|AddXHeader|A ação de política é adicionar o cabeçalho X à mensagem de email.|
|2|ModifySubject|A ação de política é modificar o assunto na mensagem de email com as informações especificadas pela política de filtragem.|
|3|Redirecionamento|A ação de política é redirecionar a mensagem de email para o endereço de email especificado pela política de filtragem.|
|4 |Excluir|A ação de política é excluir (descartar) a mensagem de email.|
|5|Quarentena|A ação de política é colocar a mensagem de email em quarentena.|
|6 |NoAction| A política está configurada para não executar nenhuma ação na mensagem de email.|
|7 |BccMessage|A ação de política é Cco a mensagem de email para o endereço de email especificado pela política de filtragem.|
|8 |ReplaceAttachment|A ação de política é substituir o anexo na mensagem de email com as informações especificadas pela política de filtragem.|
||||

### <a name="url-time-of-click-events"></a>Eventos de momento do clique da URL

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|UserId|Edm.String|Sim|O identificador (por exemplo, endereço de email) do usuário que clicou na URL.|
|AppName|Edm.String|Sim|O serviço do Office 365 em que a URL foi clicada (por exemplo, o Email).|
|URLClickAction|Automaticamente. [URLClickAction](#urlclickaction)|Sim|Clique na ação para o URL com base nas políticas da organização para [Links Seguros no Defender para Office 365](/office365/securitycompliance/atp-safe-links).|
|SourceId|Edm.String|Sim|O identificador do serviço do Office 365 em que a URL foi clicada (por exemplo, para o Email, essa é a ID de Mensagens da Rede do Exchange Online).|
|TimeOfClick|Edm.Date|Sim|A data e hora no Tempo Universal Coordenado (UTC) de quando o usuário clicou na URL.|
|URL|Edm.String|Sim|URL clicada pelo usuário.|
|UserIp|Edm.String|Sim|O endereço IP do usuário que clicou na URL. O endereço IP é exibido em um formato de endereço IPv4 ou IPv6.|
|||||

### <a name="enum-urlclickaction---type-edmint32"></a>Enumeração: URLClickAction – Tipo: Edm.Int32

#### <a name="urlclickaction"></a>URLClickAction

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|2|Blockpage|Usuário impedido de navegar para o URL pelo serviço [Links Seguros no Defender para Office 365](/office365/securitycompliance/atp-safe-links).|
|3|PendingDetonationPage|É apresentado ao Usuário uma página de detonação pendente pelo serviço [Links Seguros no Defender para o Office 365](/office365/securitycompliance/atp-safe-links).|
|4 |BlockPageOverride|O usuário é impedido de navegar para o URL pelo serviço [Links Seguros no Defender para Office 365](/office365/securitycompliance/atp-safe-links); entretanto, o usuário ultrapassa o bloqueio para navegar para a URL|
|5|PendingDetonationPageOverride|É apresentado ao Usuário uma página de detonação pelo serviço [Links Seguros no Defender para o Office 365](/office365/securitycompliance/atp-safe-links); entretanto, o usuário ultrapassa  o bloqueio para navegar até o URL.|
|||||

### <a name="file-events"></a>Eventos de arquivo

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|FileData|Self.[FileData](#filedata)|Sim|Dados sobre o arquivo que disparou o evento.|
|SourceWorkload|Self.[SourceWorkload](#sourceworkload)|Sim|Carga de trabalho ou serviço em que o arquivo foi encontrado (por exemplo, SharePoint Online, OneDrive for Business ou Microsoft Teams)
|DetectionMethod|Edm.String|Sim|O método ou tecnologia usada pelo Microsoft Defender para Office 365 para a detecção.|
|LastModifiedDate|Edm.Date|Sim|Data e hora em UTC (Tempo Universal Coordenado), na qual o arquivo foi criado ou modificado pela última vez.|
|LastModifiedBy|Edm.String|Sim|O identificador (por exemplo, um endereço de email) do usuário que criou ou modificou pela última vez o arquivo.|
|EventDeepLink|Edm.String|Sim|Link profundo para o evento de arquivo no Explorador ou para relatórios em tempo real no Centro de Conformidade e Segurança.|
|||||

### <a name="filedata-complex-type"></a>Tipo complexo FileData

#### <a name="filedata"></a>FileData

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|DocumentId|Edm.String|Sim|O identificador exclusivo do arquivo nas plataformas SharePoint, OneDrive ou Microsoft Teams.|
|FileName|Edm.String|Sim|Nome do arquivo que disparou o evento.|
|FilePath|Edm.String|Sim|Caminho (local) do arquivo no SharePoint, OneDrive ou Microsoft Teams.|
|FileVerdict|Self.[FileVerdict](#fileverdict)|Sim|O veredicto de malware do arquivo.|
|MalwareFamily|Edm.String|Não|A família de malware do arquivo.|
|SHA256|Edm.String|Sim|O hash SHA256 do arquivo.|
|FileSize|Edm.String|Sim|Tamanho do arquivo em bytes.|
|||||

### <a name="enum-sourceworkload---type-edmint32"></a>Enumeração: SourceWorkload – Tipo: Edm.Int32

#### <a name="sourceworkload"></a>SourceWorkload

|**Valor**|**Nome do membro**|
|:-----|:-----|
|0|SharePoint Online|
|1|OneDrive for Business|
|2|Microsoft Teams|
|||||

## <a name="submission-schema"></a>Esquema de envio

Os eventos de [envio](/microsoft-365/security/office-365-security/report-junk-email-messages-to-microsoft) estão disponíveis para todos os [clientes do Office 365, pois vêm com segurança](/microsoft-365/security/office-365-security/overview). Isso inclui organizações que usam a Proteção do Exchange Online e o Microsoft Defender para Office 365. Cada evento no feed de envio corresponde a falsos positivos ou falsos negativos que foram enviados como:

- **Envio do administrador**. Mensagens, arquivos ou URLs enviados à Microsoft para análise.
- **Item relatado pelo usuário**. Mensagens relatadas pelos usuários finais ao administrador ou à Microsoft para análise.

### <a name="submission-events"></a>Eventos de envio

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|AdminSubmissionRegistered|Edm.String|Não|O envio do administrador está registrado e está pendente para processamento.|
|AdminSubmissionDeliveryCheck|Edm.String|Não|O sistema de envio do administrador verificou a política do email.|
|AdminSubmissionSubmitting|Edm.String|Não|O sistema de envio do administrador está enviando o email.|
|AdminSubmissionSubmitted|Edm.String|Não|O sistema de envio do administrador enviou o email.|
|AdminSubmissionTriage|Edm.String|Não|O envio do administrador é classificado pelo avaliador.|
|AdminSubmissionTimeout|Edm.String|Não|O envio do administrador atingiu o tempo limite sem nenhum resultado.|
|UserSubmission|Edm.String|Não|O envio foi relatado pela primeira vez por um usuário final.|
|UserSubmissionTriage|Edm.String|Não|O envio do usuário é classificado pelo avaliador.|
|CustomSubmission|Edm.String|Não|A mensagem relatada por um usuário foi enviada à caixa de correio personalizada da organização, conforme definido nas configurações de mensagens de relatório do usuário.|
|AttackSimUserSubmission|Edm.String|Não|A mensagem relatada pelo usuário era, na verdade, uma mensagem de treinamento de simulação de phishing.|
|AdminSubmissionTablAllow|Edm.String|Não|Uma permissão foi criada no momento do envio para executar a ação imediatamente em mensagens semelhantes enquanto ela está sendo verificada novamente.|
|SubmissionNotification|Edm.String|Não|Os comentários da administração são enviados para o usuário final.|
|||||

## <a name="automated-investigation-and-response-events-in-office-365"></a>Eventos de investigação e resposta automatizadas no Office 365

Os eventos de [investigação e resposta automatizada do Office 365 (AIR)](/office365/securitycompliance/automated-investigation-response-office) estão disponíveis para clientes do Office 365 que possuem uma assinatura que inclui o Plano 2 do Microsoft Defender para Office 365 ou Office 365 E5. Os eventos de investigação são registrados com base em uma alteração no status de investigação. Por exemplo, quando um administrador realiza uma ação que altera o status de uma investigação de Ações Pendentes para Concluídas, um evento é registrado.

No momento, somente investigações automatizadas são registradas. (Eventos de investigações geradas manualmente serão disponibilizados em breve.) Os seguintes valores de status são registrados:

- Investigação Iniciada
- Nenhuma ameaça encontrada
- Encerrado pelo sistema
- Ação Pendente
- Ameaça Encontrada
- Remediado
- Falhou
- Encerrado pela limitação
- Encerrado Pelo Usuário
- Em execução

### <a name="main-investigation-schema"></a>Esquema de investigação principal

|Nome    |Tipo    |Descrição  |
|----|----|----|
|InvestigationId    |Edm.String    |Investigação ID/GUID |
|InvestigationName    |Edm.String    |Nome da investigação |
|InvestigationType    |Edm.String    |Tipo da investigação. Pode ter um dos seguintes valores:<br/>- Mensagens relatadas pelo usuário<br/>- Malware com Limpeza Automática Zero Hora<br/>- Phishing com Limpeza Automática Zero Hora<br/>- Alteração de Veredito de URL<p>(Investigações manuais não estão disponíveis no momento. Serão disponibilizadas em breve.) |
|LastUpdateTimeUtc    |Edm.Date    |Hora UTC da última atualização para uma investigação |
|StartTimeUtc    |Edm.Date    |Hora de início para uma investigação |
|Status     |Edm.String     |Estado de investigação, Execução, Ações Pendentes, etc. |
|DeeplinkURL    |Edm.String    |URL do link profundo para uma investigação no Centro de Conformidade & Segurança do Office 365 |
|Ações |Coleção (Edm.String)    |Conjunto de ações recomendadas por uma investigação |
|Dados    |Edm.String    |Cadeia de dados que contém mais detalhes sobre entidades de investigação e informações sobre alertas relacionados à investigação. As entidades estão disponíveis em um nó separado no blob de dados. |
||||

### <a name="actions"></a>Ações

|Campo    |Tipo    |Descrição |
|----|----|----|
|ID     |Edm.String    |ID da ação|
|ActionType    |Edm.String    |O tipo de ação, como a correção de email |
|ActionStatus    |Edm.String    |Os valores incluem: <br/>- Pendente<br/>- Executando<br/>- Aguardando o recurso<br/>- Concluído<br/>- Falhou |
|ApprovedBy    |Edm.String    |Nulo se for aprovado automaticamente; caso contrário, o nome de usuário/ID (isso será lançado em breve) |
|TimestampUtc    |Edm.DateTime    |O carimbo de data/hora da alteração do status da ação |
|ActionId    |Edm.String    |Identificador exclusivo da ação. |
|InvestigationId    |Edm.String    |Identificador exclusivo da investigação. |
|RelatedAlertIds    |Collection(Edm.String)    |Alertas relacionados a uma investigação |
|StartTimeUtc    |Edm.DateTime    |Carimbo de data/hora da criação da ação |
|EndTimeUtc    |Edm.DateTime    |Carimbo de data/hora de atualização do status final da ação |
|Identificadores de recursos     |Edm.String     |Consiste na ID de locatário do Azure Active Directory.|
|Entidades    |Collection(Edm.String)    |Lista de uma ou mais entidades afetadas por ação |
|IDs de Alertas Relacionados    |Edm.String    |Alerta relacionado a uma investigação |
||||

### <a name="entities"></a>Entidades

#### <a name="mailmessage-email"></a>MailMessage

|Campo    |Tipo    |Descrição  |
|----|----|----|
|Tipo    |Edm.String    |"mail-message"  |
|Arquivos    |Coleção (Self.File) |Detalhes dos arquivos dos anexos da mensagem |
|Destinatário    |Edm.String    |O destinatário da mensagem de email |
|URLs    |Collection(Self.URL) |Os URLs contidos na mensagem de email  |
|Remetente    |Edm.String    |O endereço de email do remetente.  |
|SenderIp    |Edm.String    |O endereço de IP do remetente.  |
|ReceivedDate    |Edm.DateTime    |A data de recebimento da mensagem  |
|NetworkMessageId    |Edm.Guid     |A ID de mensagem da rede da mensagem de email  |
|InternetMessageId    |Edm.String  |A ID de mensagem da internet da mensagem de email |
|Assunto    |Edm.String    |O assunto da mensagem de email  |
||||

#### <a name="ip"></a>IP

|Campo    |Tipo    |Descrição  |
|----|----|----|
|Tipo    |Edm.String    |"ip" |
|Endereço    |Edm.String    |O endereço IP como uma cadeia de caracteres, como, por exemplo, `127.0.0.1`
||||

#### <a name="url"></a>URL

|Campo    |Tipo    |Descrição  |
|----|----|----|
|Tipo    |Edm.String    |"url" |
|Url    |Edm.String    |A URL completa para a qual uma entidade aponta  |
||||

#### <a name="mailbox-also-equivalent-to-the-user"></a>Caixa de correio (também equivalente ao usuário) 

|Campo    |Tipo    |Descrição |
|----|----|----|
|Tipo    |Edm.String    |“Caixa de correio”  |
|MailboxPrimaryAddress    |Edm.String    |O endereço principal da caixa de correio  |
|DisplayName    |Edm.String    |O nome de exibição da caixa de correio. |
|UPN    |Edm.String    |UPN da caixa de correio  |
||||

#### <a name="file"></a>Arquivo

|Campo    |Tipo    |Descrição  |
|----|----|----|
|Tipo    |Edm.String    |“Arquivo” |
|Nome    |Edm.String    |O nome do arquivo sem caminho |
FileHashes |Coleção (Edm.String)    |Os hashes de arquivo estão associado ao arquivo. |
||||

#### <a name="filehash"></a>FileHash

|Campo    |Tipo    |Descrição |
|----|----|----|
|Tipo    |Edm.String    |"filehash" |
|Algoritmo    |Edm.String    |O tipo de algoritmo hash, que pode ser um destes valores:<br/>- Desconhecido<br/>- MD5<br/>- SHA1<br/>- SHA256<br/>- SHA256AC
|Valor    |Edm.String    |O valor do hash  |
||||

#### <a name="mailcluster"></a>MailCluster

|Campo    |Tipo    |Descrição   |
|----|----|----|
|Tipo    |Edm.String    |"MailCluster" <br/>Determina o tipo de entidade discutida |
|NetworkMessageIds    |Coleção (Edm.String)    |Lista das IDs de mensagem de email que fazem parte do cluster de emails |
|CountByDeliveryStatus    |Coleções (Edm.String)    |Contagem de mensagens de email por cadeia de caracteres DeliveryStatus |
|CountByThreatType    |Coleções (Edm.String) |Contagem de mensagens de email por cadeia de caracteres ThreatType |
|Ameaças    |Coleções (Edm.String)    |As ameaças de mensagens de email que fazem parte do cluster de emails. As ameaças incluem valores como Phish e Malware. |
|Consulta    |Edm.String    |A consulta usada para identificar as mensagens do cluster de emails  |
|QueryTime    |Edm.DateTime    |O tempo de consulta  |
|MailCount    |Edm.int    |O número de mensagens de email que fazem parte do cluster de emails  |
|Origem    |Cadeia de Caracteres    |A origem do cluster de email; o valor da origem do cluster. |
||||

## <a name="hygiene-events-schema"></a>Esquema de eventos de higiene

Os eventos de higiene estão relacionados à proteção contra spam de saída. Esses eventos estão relacionados a usuários impedidos de enviar email. Para saber mais, confira:

- [Proteção contra spam de saída](/microsoft-365/security/office-365-security/outbound-spam-controls)

- [Remover usuários bloqueados do portal de Usuários Restritos no Office 365](/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam)

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Auditoria|Edm.String|Não|Informações do sistema relacionadas ao evento de higiene.|
|Evento|Edm.String|Não|O tipo de evento de higiene. Os valores para este parâmetro são **Listado** ou **Removido**.|
|EventId|Edm.Int64|Não|O ID do tipo de evento de higiene.|
|EventValue|Edm.String|Não|O usuário que foi impactado.|
|Reason|Edm.String|Não|Detalhes sobre o evento de higiene.|
|||||

## <a name="power-bi-schema"></a>Esquema do Power BI

Os eventos do Power BI listados em [Pesquisar o log de auditoria no Centro de Proteção do Office 365](/power-bi/service-admin-auditing#activities-audited-by-power-bi) usarão este esquema.

|**Parameters**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
| AppName               | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | O nome do aplicativo em que o evento ocorreu. |
| DashboardName         | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | O nome do painel onde o evento ocorreu. |
| DataClassification    | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | As [classificação dos dados](/power-bi/service-data-classification), se houver, do painel onde o evento ocorreu. |
| DatasetName           | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | O nome do conjunto de dados onde o evento ocorreu. |
| MembershipInformation | Conjunto ([MembershipInformationType](#membershipinformationtype-complex-type)) Term="Microsoft.Office.Audit.Schema.PIIFlag" booleano = "true" |  Não  | Informações de associação sobre o grupo. |
| OrgAppPermission      | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | Lista de permissões de um aplicativo organizacional (toda a organização, usuários específicos ou grupos específicos). |
| NomeDoRelatório            | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | O nome do relatório em que o evento ocorreu. |
| SharingInformation    | Conjunto ([SharingInformationType](#sharinginformationtype-complex-type)) Term="Microsoft.Office.Audit.Schema.PIIFlag" booleano = "true"    |  Não  | Informações sobre a pessoa para quem é enviado um convite de compartilhamento. |
| SwitchState           | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | Informações sobre o estado das várias opções de nível do locatário. |
| WorkSpaceName         | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  Não  | O nome do espaço de trabalho em que o evento ocorreu. |
|||||

### <a name="membershipinformationtype-complex-type"></a>Tipo de complexo MembershipInformationType

|**Parameters**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
| MemberEmail | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  Não  | O endereço de email do grupo. |
| Status      | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  Não  | Não está preenchido no momento. |
|||||

### <a name="sharinginformationtype-complex-type"></a>SharingInformationType tipo complexo

|**Parameters**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
| RecipientEmail    | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  Não  | O endereço de email do destinatário de um convite de compartilhamento. |
| RecipientName    | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  Não  | O nome do destinatário de um convite de compartilhamento. |
| ResharePermission | Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  Não  | A permissão sendo concedida ao destinatário. |
|||||

## <a name="dynamics-365-schema"></a>Esquema do Dynamics 365

Os registros de auditoria para eventos relacionados aos aplicativos controlados por modelos no Dynamics 365 usam um esquema de operações de entidade e base. Para obter mais informações, consulte [Ativar e usar o Log de Atividade](/power-platform/admin/enable-use-comprehensive-auditing#model-driven-apps-in-dynamics-365-schema).

### <a name="dynamics-365-base-schema"></a>Esquema base do Dynamics 365

| **Parâmetros**     | **Tipo**            | **Obrigatório?** | **Descrição**|
|:------------------ | :------------------ | :--------------|:--------------|
|CrmOrganizationUniqueName|Edm.String|Sim|O nome exclusivo da organização.|
|InstanceUrl|Edm.String|Sim|A URL da instância.|
|ItemUrl|Edm.String|Não|A URL do registro que emite o log.|
|ItemType|Edm.String|Não|O nome da entidade.|
|UserAgent|Edm.String|Não|O identificador exclusivo da GUID de usuário na organização.|
|Campos|Collection(Common.NameValuePair)|Não|Um objeto JSON que contém os pares da propriedade chave-valor criados ou atualizados.|
|||||

### <a name="dynamics-365-entity-operation-schema"></a>Esquema de operações de entidade do Dynamics 365

Eventos de entidade de aplicativos controlados por modelos no Dynamics 365 use este esquema para criar o esquema base do Dynamics 365. Esse esquema inclui informações sobre a operação de entidade que disparou o evento auditado.

| **Parâmetros**     | **Tipo**            | **Obrigatório?** | **Descrição**|
|:------------------ | :------------------ | :--------------|:--------------|
|EntityId|Edm.Guid|Não|Identificador exclusivo da entidade.|
|EntityName|Edm.String|Sim|O nome da entidade na organização. Exemplos de entidades incluem `contact` ou `authentication`.|
|Mensagem|Edm.String|Sim|Esse parâmetro contém a operação que foi realizada em relação à entidade. Por exemplo, se um novo contato tiver sido criado, o valor da propriedade Mensagem será `Create` e o valor correspondente da propriedade EntityName será `contact`.|
|Consulta|Edm.String|Não|Os parâmetros da consulta de filtro que foi usada durante a execução da operação FetchXML.|
|PrimaryFieldValue|Edm.String|Não|Indica o valor do atributo que é o campo principal da entidade.|
|||||

## <a name="workplace-analytics-schema"></a>Esquema do Workplace Analytics

Os eventos do WorkPlace Analytics listados em [Pesquisar o log de auditoria no Centro de Conformidade e Segurança do Office 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-workplace-analytics-activities) usarão este esquema.

| **Parâmetros**     | **Tipo**            | **Obrigatório?** | **Descrição**|
|:------------------ | :------------------ | :--------------|:--------------|
| WpaUserRole        | Edm.String | Não     | A função do Workplace Analytics do usuário que executou a ação.|
| ModifiedProperties | Coleção (Common.ModifiedProperty) | Não | Essa propriedade inclui o nome da propriedade que foi modificada, o novo valor da propriedade modificada e o valor anterior da propriedade modificada.|
| OperationDetails   | Coleção (Common.NameValuePair)    | Não | Uma lista de propriedades estendidas para a configuração que foi alterada. Cada propriedade terá um **Nome** e **Valor**.|
||||

## <a name="quarantine-schema"></a>Esquema de quarentena

Os eventos de quarentena listados em [Pesquisar o log de auditoria no Centro de Conformidade e Segurança do Office 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#quarantine-activities) usarão este esquema. Para saber mais sobre a quarentena, confira [Mensagens de email em quarentena no Office 365](/microsoft-365/security/office-365-security/quarantine-email-messages).

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|RequestType|Self.[RequestType](#enum-requesttype---type-edmint32)|Não|O tipo de solicitação de quarentena executada por um usuário.|
|RequestSource|Self.[RequestSource](#enum-requestsource---type-edmint32)|Não|A origem de uma solicitação de quarentena pode vir do Centro de Conformidade e Segurança (SCC), de um cmdlet ou de um URLlink.|
|NetworkMessageId|Edm.String|Não|A ID da mensagem de rede da mensagem de email em quarentena.|
|ReleaseTo|Edm.String|Não|O destinatário da mensagem de email.|
|||||

### <a name="enum-requesttype---type-edmint32"></a>Enum: RequestType - Type: Edm.Int32

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|Visualização|Esta é uma solicitação de um usuário para visualizar uma mensagem de email considerada prejudicial.|
|1|Excluir|Esta é uma solicitação de um usuário para excluir uma mensagem de email considerada prejudicial.|
|2|Liberar|Esta é uma solicitação de um usuário para liberar uma mensagem de email considerada prejudicial.|
|3|Exportar|Esta é uma solicitação de um usuário para exportar uma mensagem de email considerada prejudicial.|
|4 |ViewHeader|Esta é uma solicitação de um usuário para exibir o cabeçalho de uma mensagem de email considerada prejudicial.|
|5|Solicitação de liberação|Esta é uma solicitação de liberação de um usuário para liberar uma mensagem de email considerada prejudicial.|
||||

### <a name="enum-requestsource---type-edmint32"></a>Enum: RequestSource - Type: Edm.Int32

|**Valor**|**Nome do membro**|**Descrição**|
|:-----|:-----|:-----|
|0|SCC|O Centro de Conformidade e Segurança (SCC) é a fonte da qual pode originar a solicitação de um usuário para visualizar, excluir, liberar, exportar ou exibir o cabeçalho de uma mensagem de email potencialmente prejudicial. |
|1|Cmdlet|Um cmdlet é a fonte da qual pode originar a solicitação de um usuário para visualizar, excluir, liberar, exportar ou exibir o cabeçalho de uma mensagem de email potencialmente prejudicial.|
|2|URLlink|Essa é a fonte da qual pode originar a solicitação de um usuário para visualizar, excluir, liberar, exportar ou exibir o cabeçalho de uma mensagem de email potencialmente prejudicial.|
||||

## <a name="microsoft-forms-schema"></a>Esquema do Microsoft Forms

Os eventos do Microrosft Forms listados em [Pesquisar o log de auditoria no Centro de Segurança e Conformidade do Office 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-forms-activities) usarão este esquema.

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|FormsUserTypes|Coleção (Self.[FormsUserTypes](#formsusertypes))|Sim|O papel do usuário que executou a ação.  Os valores desse parâmetro são Administrador, Proprietário, Responder ou Coautoria.|
|SourceApp|Edm.String|Sim|Indica se a ação é do site do Forms ou de outro aplicativo.|
|FormName|Edm.String|Não|Nome do formulário atual.|
|FormId |Edm.String|Não|A ID do formulário de destino.|
|FormTypes|Coleção (Self.[FormTypes](#formtypes))|Não|Indica se isso é um Formulário, Quiz ou Pesquisa.|
|ActivityParameters|Edm.String|Não|Cadeia de caracteres JSON contendo parâmetros de atividade. Confira [Pesquisar no log de auditoria no Centro de Conformidade e Segurança do Office 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#microsoft-forms-activities) para saber mais.|
||||

### <a name="enum-formsusertypes---type-edmint32"></a>Enum: FormsUserTypes - Tipo: Edm.Int32

#### <a name="formsusertypes"></a>FormsUserTypes

|**Valor**|**Tipo de Usuário do Formulário**|**Descrição**|
|:-----|:-----|:-----|
|0|Administrador|Um administrador que tem acesso ao formulário.|
|1|Proprietário|Um usuário que é o proprietário do formulário.|
|2|Respondente|Um usuário que enviou uma resposta a um formulário.|
|3|Co-autor|Um usuário que usou um link de colaboração fornecido pelo proprietário do formulário para fazer logon e editar um formulário.|
||||

### <a name="enum-formtypes---type-edmint32"></a>Enum: FormTypes - Tipo: Edm.Int32

#### <a name="formtypes"></a>FormTypes

|**Valor**|**Tipos de Formulário**|**Descrição**|
|:-----|:-----|:-----|
|0|Formulário|Formulários criados com a opção Novo Formulário.|
|1|Quiz|Quizzes criados com a Nova Opção de Quiz.  Um quiz é um tipo especial de formulário que inclui recursos adicionais como valores de ponto, classificações automáticas e manuais e comentários.|
|2|Pesquisa|Pesquisas criadas com a opção Nova Pesquisa.  Uma pesquisa é um tipo especial de formulário que inclui recursos adicionais como a integração e o suporte a CMS para regras de Fluxo.|
||||

## <a name="mip-label-schema"></a>Esquema de rótulos MIP

Os eventos no esquema de rótulo do Microsoft Information Protection (MIP) são disparados quando o Microsoft 365 detecta uma mensagem de email processada por agentes no pipeline de Transporte que tem um rótulo de confidencialidade aplicado a ele. O rótulo de confidencialidade pode ter sido aplicado manual ou automaticamente e pode ter sido aplicado dentro ou fora do pipeline de Transporte. Os rótulos de confidencialidade podem ser aplicados automaticamente às mensagens de email por meio da aplicação automática de políticas de rótulo.

O propósito desse esquema de auditoria é representar a soma de toda a atividade de email que envolve rótulos de confidencialidade. Em outras palavras, deve haver uma atividade de auditoria redirecionada para cada mensagem de email que será enviada para ou a partir de usuários na organização que tenha um rótulo de confidencialidade aplicado a ele, independentemente de quando ou como o rótulo de sensibilidade tenha sido aplicado. Para obter mais informações sobre rótulos de confidencialidade, confira:

- [Saiba mais sobre rótulos de confidencialidade](/microsoft-365/compliance/sensitivity-labels)

- [Aplicar um rótulo de confidencialidade automaticamente ao conteúdo](/microsoft-365/compliance/apply-sensitivity-label-automatically)

|**Parâmetros**|**Tipo**|**Obrigatório?**|**Descrição**|
|:-----|:-----|:-----|:-----|
|Remetente|Edm.String|Não|O endereço de email no campo De da mensagem de email.|
|Receptores|Collection(Edm.String)|Não|Todos os endereços de email nos campos Para, CC e Cco da mensagem de email.|
|ItemName|Edm.String|Não|A cadeia de caracteres no campo Assunto da mensagem de email.|
|LabelID|Edm.Guid|Não|O GUID do rótulo de confidenciallidade aplicado à mensagem de email.|
|LabelName|Edm.String|Não|O nome do rótulo de sensibilidade aplicado à mensagem de email.|
|Labelaction|Edm.String|Não|As ações especificadas pelo rótulo de confidencialidade que foram aplicados à mensagem de email antes da mensagem entrar no pipeline de transporte de email.|
|LabelAppliedDateTime|Edm.Date|Não|A data em que o rótulo de confidencialidade foi aplicado à mensagem de email.|
|Applicationmode|Edm.String|Não|Especifica como o rótulo de confidencialidade foi aplicado à mensagem de email. O valor **Privilegiado** indica que o rótulo foi aplicado manualmente por um usuário. O valor **Padrão** indica que o rótulo foi aplicado automaticamente por um processo de rotulagem do lado do cliente ou do serviço.|
|||||

## <a name="communication-compliance-exchange-schema"></a>Esquema de conformidade de comunicações do Exchange

Os eventos de conformidade de comunicação listados no log de auditoria do Office 365 usam esse esquema. Isso inclui registros de auditoria para a operação **SupervisoryReviewOLAudit** gerados quando o conteúdo da mensagem de email contém uma linguagem ofensiva identificada por modelos antispam com uma precisão de correspondência de \>= 99,5%.

|**Parâmetros**  |**Tipo**|**Obrigatório?** |**Descrição**|
|:---------------|:-------|:--------------|:--------------|
| ExchangeDetails |[ExchangeDetails](#exchangedetails)|Não|Propriedades da mensagem de email que disparou o evento SupervisoryReviewOLAudit.|
|||||

### <a name="enum-exchangedetails---type-exchangedetails"></a>Enum: ExchangeDetails - Type: ExchangeDetails

#### <a name="exchangedetails"></a>ExchangeDetails

| **Nome do membro**   | **Tipo**| **Descrição**|
|:----------------- | :-------|:---------------|
| NetworkMessageId  |Edm.Guid|A ID de mensagem da rede da mensagem de email.|
| InternetMessageId |Edm.String|A ID de mensagem da internet da mensagem de email.|
| AttachmentData|Collection([AttachmentDetails](#attachmentdetails))|Informações sobre arquivos anexados à mensagem de emails.|
| Destinatários|Collection(Edm.String)|Todos os endereços de email nos campos Para, CC e Cco da mensagem de email. |
| Assunto|Edm.String|O texto no campo Assunto da mensagem de email.|
| MessageTime|Edm.Date|A data e a hora em que a mensagem foi enviada.|
| De| Edm.String|O endereço de email no campo De da mensagem de email.|
| Directionality|Edm.String|O status da origem da mensagem de email.|
||||

### <a name="enum-attachmentdetails---type-edmint32"></a>Enum: AttachmentDetails - Type: Edm.Int32

#### <a name="attachmentdetails"></a>AttachmentDetails

| **Nome do membro** | **Tipo**   | **Descrição**|
|:--------------- |:---------- | :--------------|
| FileName        | Edm.String | O nome do arquivo anexado à mensagem de email.|
| FileType        | Edm.String | A extensão de arquivo do arquivo anexado à mensagem de email.|
| SHA256          | Edm.String | O hash SHA-256 do arquivo anexado à mensagem de email.|
||||

## <a name="reports-schema"></a>Esquema de relatórios

Os eventos de quarentena listados em [Pesquisar o log de auditoria no Centro de Segurança e Conformidade do Office 365](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#reports-activities) usarão este esquema.

|**Parameters**  |**Tipo**|**Obrigatório?** |**Descrição**|
|:---------------|:-------|:--------------|:--------------|
| ModifiedProperties | Coleção (Common.ModifiedProperty) | Não | Essa propriedade inclui o nome da propriedade que foi modificada, o novo valor da propriedade modificada e o valor anterior da propriedade modificada.|
|||||
