---
title: Configurar o fluxo de credenciais de senha de proprietário do recurso no Azure AD B2C | Microsoft Docs
description: Saiba como configurar o fluxo de credenciais de senha de proprietário do recurso no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/24/2018
ms.author: davidmu
ms.openlocfilehash: c1b4d641f6830751e2cb9e66d5052eb20a48d371
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34158247"
---
# <a name="configure-the-resource-owner-password-credentials-flow-ropc-in-azure-ad-b2c"></a>Configure o fluxo de credenciais de senha de proprietário do recurso (ROPC) no Azure AD B2C

O fluxo de credenciais de senha de proprietário do recurso (ROPC) é um fluxo de autenticação padrão do OAUTH em que o aplicativo, também conhecido como a terceira parte confiável, troca credenciais válidas, como ID de usuário e senha para um token de ID, o token de acesso e um token de atualização. 

> [!NOTE]
> Esse recurso está em visualização.

No Azure AD B2C, estas opções são suportadas:

- **Cliente Nativo** – A interação do usuário durante a autenticação ocorre com o uso do código em execução em um dispositivo de usuário, que pode ser um aplicativo móvel em execução no sistema operacional nativo, como Android, ou em execução no navegador, como Javascript.
- **Fluxo de cliente público** – Somente credenciais de usuário, coletadas por um aplicativo, são enviadas na chamada de API. As credenciais do aplicativo não são enviadas.
- **Adicionar novas declarações** -O conteúdo do token de ID pode ser alterado para adicionar novas declarações. 

Estes fluxos não são suportados:

- **Servidor-para-servidor** O sistema de proteção de identidade (IDPS) precisa de um endereço IP confiável coletado pelo chamador (o cliente nativo) como parte da interação.  Em uma chamada de API do lado do servidor, somente o endereço IP do servidor é usado, e o IDPS pode identificar um endereço IP repetido como um invasor se um limite dinâmico de autenticações com falha for excedido.
- **Fluxo de cliente confidencial** - A ID do cliente do aplicativo é validada, mas o segredo do aplicativo não é validado.

##  <a name="create-a-resource-owner-policy"></a>Criar uma política de proprietário de recurso

1. Entre no portal do Azure como administrador global do locatário Azure AD B2C.
2. Para alternar para seu locatário do Azure AD B2C, selecione o diretório do B2C no canto superior direito do portal.
3. Em **Políticas**, selecione **Políticas de Proprietário do Recurso**.
4. Forneça um nome para a política, como *ROPC_Auth* e, em seguida, clique em **Declarações de aplicativo**.
5. Selecione as declarações de aplicativo de que você precisa para seu aplicativo, como *Nome de Exibição*, *Endereço de Email*, e *Provedor de Identidade*.
6. Clique em **OK** e depois clique em **Criar**.

Você verá um ponto de extremidade, como neste exemplo:

`https://login.microsoftonline.com/yourtenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1A_ROPC_Auth`


## <a name="register-an-application"></a>Registrar um aplicativo

1. Nas configurações de B2C, clique em **Aplicativos** e depois em **+Adicionar**.
2. Forneça um nome para o aplicativo, como *ROPC_Auth_app*.
3. Clique em **Não** para **Aplicativo Web/API da Web** e clique em **Sim** para **Cliente nativo**.
4. Deixe todos os outros valores como estão e clique em **Criar**.
5. Selecione o novo aplicativo e anote a ID do aplicativo.

## <a name="test-the-policy"></a>Testar a política

Use seu aplicativo favorito de desenvolvimento de API para gerar uma chamada de API e analise a resposta para depurar sua política. Construa uma chamada como esta com as informações na tabela a seguir como o corpo da solicitação POST:
- Substitua *yourtenant.onmicrosoft.com* com o nome do seu locatário B2C
- Substitua *B2C_1A_ROPC_Auth* com o nome completo da sua política ROPC
- Substitua *bef2222d56-552f-4a5b-b90a-1988a7d634c3* pela ID do aplicativo de seu registro.

`https://te.cpim.windows.net/yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

| Chave | Valor |
| --- | ----- |
| Nome de Usuário | leadiocl@outlook.com |
| Senha | Passxword1 |
| grant_type | Senha |
| scope | openid bef2222d56-552f-4a5b-b90a-1988a7d634c3 offline_access |
| client_id | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| response_type | token id_token |

*Client_id* é o valor que você anotou anteriormente como a ID do aplicativo. *Offline_access* será opcional se você desejar receber um token de atualização. 

A solicitação POST real tem esta aparência:

```
POST /yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token HTTP/1.1
Host: te.cpim.windows.net
Content-Type: application/x-www-form-urlencoded

username=leadiocl%40trashmail.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```


Uma resposta bem-sucedida com acesso offline se assemelha ao seguinte exemplo:

```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJpc3MiOiJodHRwczovL3RlLmNwaW0ud2luZG93cy5uZXQvZjA2YzJmZTgtNzA5Zi00MDMwLTg1ZGMtMzhhNGJmZDllODJkL3YyLjAvIiwiZXhwIjoxNTEzMTMwMDc4LCJuYmYiOjE1MTMxMjY0NzgsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsIm9pZCI6IjNjM2I5NjljLThjNDktNGUxMS1hNGVmLWZkYjJmMzkyZjA0OSIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF6cCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsInZlciI6IjEuMCIsImlhdCI6MTUxMzEyNjQ3OH0.MSEThYZxCS4SevBw3-3ecnVLUkucFkehH-gH-P7SFcJ-MhsBeQEpMF1Rzu_R9kUqV3qEWKAPYCNdZ3_P4Dd3a63iG6m9TnO1Vt5SKTETuhVx3Xl5LYeA1i3Slt9Y7LIicn59hGKRZ8ddrQzkqj69j723ooy01amrXvF6zNOudh0acseszt7fbzzofyagKPerxaeTH0NgyOinLwXu0eNj_6RtF9gBfgwVidRy9OzXUJnqm1GdrS61XUqiIUtv4H04jYxDem7ek6E4jsH809uSXT0iD5_4C5bDHrpO1N6pXSasmVR9GM1XgfXA_IRLFU4Nd26CzGl1NjbhLnvli2qY4A", 
    "token_type": "Bearer", 
    "expires_in": "3600", 
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6IjEuMCJ9.aJ_2UW14dh4saWTQ0jLJ7ByQs5JzIeW_AU9Q_RVFgrrnYiPhikEc68ilvWWo8B20KTRB_s7oy_Eoh5LACsqU6Oz0Mjnh0-DxgrMblUOTAQ9dbfAT5WoLZiCBJIz4YT5OUA_RAGjhBUkqGwdWEumDExQnXIjRSeaUBmWCQHPPguV1_5wSj8aW2zIzYIMbofvpjwIATlbIZwJ7ufnLypRuq_MDbZhJkegDw10KI4MHJlJ40Ip8mCOe0XeJIDpfefiJ6WQpUq4zl06NO7j8kvDoVq9WALJIao7LYk_x9UIT-3d0W0eDBHGSRcNgtMYpymaN9ltx6djcEesXNn4CFnWG3g.y6KKeA9EcsW9zW-g.TrTSgn4WBt18gezegxihBla9SLSTC3YfDROQsL9K4yX4400FKlTlf-2l9CnpGTEdWXVi7sIMHCl8S4oUiXd-rvY2mn_NfDrbbVJfgKp1j7Nnq9FFyeJEFcP_FtUXgsNTG9iwfzWox04B1d845qNRWiS9N8BhAAAIdz5N0ChHuOxsVOC0Y_Ly3DNe-JQyXcq964M6-jp3cgi4UqMxT837L6pLY5Ih_iPsSfyHzstsFeqyUIktnzt1MpTlyW-_GDyFK1S-SyV8PPQ7phgFouw2jho1iboHX70RlDGYyVmP1CfQzKE_zWxj3rgaCZvYMWN8fUenoiatzhvWkUM7dhqKGjofPeL8rOMkhl6afLLjObzhUg3PZFcMR6guLjQdEwQFufWxGjfpvaHycZSKeWu6-7dF8Hy_nyMLLdBpUkdrXPob_5gRiaH72KvncSIFvJLqhY3NgXO05Fy87PORjggXwYkhWh4FgQZBIYD6h0CSk2nfFjR9uD9EKiBBWSBZj814S_Jdw6HESFtn91thpvU3hi3qNOi1m41gg1vt5Kh35A5AyDY1J7a9i_lN4B7e_pknXlVX6Z-Z2BYZvwAU7KLKsy5a99p9FX0lg6QweDzhukXrB4wgfKvVRTo.mjk92wMk-zUSrzuuuXPVeg", 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlkifQ.eyJleHAiOjE1MTMxMzAwNzgsIm5iZiI6MTUxMzEyNjQ3OCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly90ZS5jcGltLndpbmRvd3MubmV0L2YwNmMyZmU4LTcwOWYtNDAzMC04NWRjLTM4YTRiZmQ5ZTgyZC92Mi4wLyIsInN1YiI6Ik5vdCBzdXBwb3J0ZWQgY3VycmVudGx5LiBVc2Ugb2lkIGNsYWltLiIsImF1ZCI6ImJlZjIyZDU2LTU1MmYtNGE1Yi1iOTBhLTE5ODhhN2Q2MzRjZSIsImFjciI6ImIyY18xYV9yZXNvdXJjZW93bmVydjIiLCJpYXQiOjE1MTMxMjY0NzgsImF1dGhfdGltZSI6MTUxMzEyNjQ3OCwib2lkIjoiM2MzYjk2OWMtOGM0OS00ZTExLWE0ZWYtZmRiMmYzOTJmMDQ5IiwiYXRfaGFzaCI6Ikd6QUNCTVJtcklwYm9OdkFtNHhMWEEifQ.iAJg13cgySsdH3cmoEPGZB_g-4O8KWvGr6W5VzRXtkrlLoKB1pl4hL6f_0xOrxnQwj2sUgW-wjsCVzMc_dkHSwd9QFZ4EYJEJbi1LMGk2lW-PgjsbwHPDU1mz-SR1PeqqJgvOqrzXo0YHXr-e07M4v4Tko-i_OYcrdJzj4Bkv7ZZilsSj62lNig4HkxTIWi5Ec2gD79bPKzgCtIww1KRnwmrlnCOrMFYNj-0T3lTDcXAQog63MOacf7OuRVUC5k_IdseigeMSscrYrNrH28s3r0JoqDhNUTewuw1jx0X6gdqQWZKOLJ7OF_EJMP-BkRTixBGK5eW2YeUUEVQxsFlUg" 
} 
```

## <a name="redeem-a-refresh-token"></a>Resgatar um token de atualização

Construa uma chamada POST como esta com as informações na tabela a seguir como o corpo da solicitação:

`https://te.cpim.windows.net/yourtenant.onmicrosoft.com/B2C_1A_ROPC_Auth/oauth2/v2.0/token`

| Chave | Valor |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| recurso | bef2222d56-552f-4a5b-b90a-1988a7d634c3 |
| refresh_token | eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3... |

*Client_id* e *resource* são os valores que você anotou anteriormente como a ID do aplicativo. *Refresh_token* é o token recebido na chamada de autenticação mencionada anteriormente.

## <a name="implement-with-your-preferred-native-sdk-or-use-app-auth"></a>Implementar com o SDK nativo preferencial ou usar App-Auth


A implementação do Azure AD B2C atende aos padrões OAuth 2.0 ou ROPC de cliente público e deve ser compatível com a maioria dos SDKs clientes.  Nós testamos esse fluxo extensivamente, em produção, com AppAuth para iOS e AppAuth para Android.  Consulte https://appauth.io/ para obter as informações mais recentes.

Você pode baixar os exemplos de funcionamento, que foram configurados para uso com o Azure AD B2C do github em https://aka.ms/aadb2cappauthropc para Android e https://aka.ms/aadb2ciosappauthropc.




