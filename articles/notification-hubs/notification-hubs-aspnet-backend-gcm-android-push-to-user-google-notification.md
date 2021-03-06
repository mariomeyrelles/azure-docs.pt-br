---
title: Notificações por push para usuários específicos de aplicativo Android usando Hubs de Notificação do Azure | Microsoft Docs
description: Saiba como enviar notificações por push para usuários específicos usando o Hubs de Notificação do Azure.
documentationcenter: android
services: notification-hubs
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: ae0e17a8-9d2b-496e-afd2-baa151370c25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/07/2018
ms.author: dimazaid
ms.openlocfilehash: b944aa84a3962e16a153bc1840e43a7f405f8437
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-notification-to-specific-android-application-users-by-using-azure-notification-hubs"></a>Tutorial: Notificação por push para usuários de aplicativo Android específico usando Hubs de Notificação do Azure
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

Este tutorial mostra como usar os Hubs de Notificação do Azure para enviar notificações por push a um usuário específico do aplicativo em um dispositivo específico. Um back-end da API Web ASP.NET é usado para autenticar clientes e gerar notificações, conforme mostrado no artigo de diretrizes [Registrando-se por meio do back-end do aplicativo](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Este tutorial baseia-se no hub de notificação que você criou no [Tutorial: Notificações por push para dispositivos Android usando Hubs de Notificação do Azure e Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md).

Neste tutorial, você deve executar as seguintes etapas: 

> [!div class="checklist"]
> * Crie o projeto de API da Web de back-end que autentica os usuários.  
> * Atualize o aplicativo Android. 
> * Testar o aplicativo

## <a name="prerequisites"></a>pré-requisitos
Concluir o [Tutorial: notificações por push para aplicativos Android usando Hubs de Notificação do Azure e Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md) antes de realizar este tutorial. 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Criar o projeto Android
A próxima etapa é atualizar o aplicativo Android que você criou no [Tutorial: Notificações por push para dispositivos Android usando Hubs de Notificação do Azure e Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md). 

1. Abra o arquivo **res/layout/activity_main.xml** e substitua pelas definições de conteúdo a seguir:
   
    Ele adiciona novos controles EditText para fazer logon como um usuário. Além disso, um campo é adicionado para uma marca username, que fará parte das notificações enviadas:
   
    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

    <EditText
        android:id="@+id/usernameText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/usernameHint"
        android:layout_above="@+id/passwordText"
        android:layout_alignParentEnd="true" />
    <EditText
        android:id="@+id/passwordText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/passwordHint"
        android:inputType="textPassword"
        android:layout_above="@+id/buttonLogin"
        android:layout_alignParentEnd="true" />
    <Button
        android:id="@+id/buttonLogin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/loginButton"
        android:onClick="login"
        android:layout_above="@+id/toggleButtonGCM"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="24dp" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="WNS on"
        android:textOff="WNS off"
        android:id="@+id/toggleButtonWNS"
        android:layout_toLeftOf="@id/toggleButtonGCM"
        android:layout_centerVertical="true" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="GCM on"
        android:textOff="GCM off"
        android:id="@+id/toggleButtonGCM"
        android:checked="true"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="APNS on"
        android:textOff="APNS off"
        android:id="@+id/toggleButtonAPNS"
        android:layout_toRightOf="@id/toggleButtonGCM"
        android:layout_centerVertical="true" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessageTag"
        android:layout_below="@id/toggleButtonGCM"
        android:layout_centerHorizontal="true"
        android:hint="@string/notification_message_tag_hint" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_below="@+id/editTextNotificationMessageTag"
        android:layout_centerHorizontal="true"
        android:hint="@string/notification_message_hint" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:onClick="sendNotificationButtonOnClick"
        android:layout_below="@+id/editTextNotificationMessage"
        android:layout_centerHorizontal="true" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:id="@+id/text_hello"
    />  
    </RelativeLayout>
    ```
3. Abra o arquivo **res/values/strings.xml** e substitua a definição `send_button` pelas linhas a seguir, que redefinem a cadeia de caracteres para o `send_button` e adicione cadeias de caracteres aos outros controles:
   
    ```xml
    <string name="usernameHint">Username</string>
    <string name="passwordHint">Password</string>
    <string name="loginButton">1. Log in</string>
    <string name="send_button">2. Send Notification</string>
    <string name="notification_message_hint">Notification message</string>
    <string name="notification_message_tag_hint">Recipient username</string>
    ```

    O layout gráfico do main_activity.xml deve ter a imagem a seguir:
   
    ![][A1]
4. Crie uma nova classe chamada **RegisterClient** no mesmo pacote que a classe `MainActivity`. Use o código a seguir para o novo arquivo de classe.

    ```java   
    import java.io.IOException;
    import java.io.UnsupportedEncodingException;
    import java.util.Set;

    import org.apache.http.HttpResponse;
    import org.apache.http.HttpStatus;
    import org.apache.http.client.ClientProtocolException;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.client.methods.HttpPut;
    import org.apache.http.client.methods.HttpUriRequest;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.DefaultHttpClient;
    import org.apache.http.util.EntityUtils;
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.util.Log;

    public class RegisterClient {
        private static final String PREFS_NAME = "ANHSettings";
        private static final String REGID_SETTING_NAME = "ANHRegistrationId";
        private String Backend_Endpoint;
        SharedPreferences settings;
        protected HttpClient httpClient;
        private String authorizationHeader;

        public RegisterClient(Context context, String backendEnpoint) {
            super();
            this.settings = context.getSharedPreferences(PREFS_NAME, 0);
            httpClient =  new DefaultHttpClient();
            Backend_Endpoint = backendEnpoint + "/api/register";
        }

        public String getAuthorizationHeader() {
            return authorizationHeader;
        }

        public void setAuthorizationHeader(String authorizationHeader) {
            this.authorizationHeader = authorizationHeader;
        }

        public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
            String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

            JSONObject deviceInfo = new JSONObject();
            deviceInfo.put("Platform", "gcm");
            deviceInfo.put("Handle", handle);
            deviceInfo.put("Tags", new JSONArray(tags));

            int statusCode = upsertRegistration(registrationId, deviceInfo);

            if (statusCode == HttpStatus.SC_OK) {
                return;
            } else if (statusCode == HttpStatus.SC_GONE){
                settings.edit().remove(REGID_SETTING_NAME).commit();
                registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                statusCode = upsertRegistration(registrationId, deviceInfo);
                if (statusCode != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            } else {
                Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                throw new RuntimeException("Error upserting registration");
            }
        }

        private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                throws UnsupportedEncodingException, IOException,
                ClientProtocolException {
            HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
            request.setEntity(new StringEntity(deviceInfo.toString()));
            request.addHeader("Authorization", "Basic "+authorizationHeader);
            request.addHeader("Content-Type", "application/json");
            HttpResponse response = httpClient.execute(request);
            int statusCode = response.getStatusLine().getStatusCode();
            return statusCode;
        }

        private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
            if (settings.contains(REGID_SETTING_NAME))
                return settings.getString(REGID_SETTING_NAME, null);

            HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
            request.addHeader("Authorization", "Basic "+authorizationHeader);
            HttpResponse response = httpClient.execute(request);
            if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                throw new RuntimeException("Error creating Notification Hubs registrationId");
            }
            String registrationId = EntityUtils.toString(response.getEntity());
            registrationId = registrationId.substring(1, registrationId.length()-1);

            settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

            return registrationId;
        }
    }
    ```
       
    Esse componente implementa as chamadas do REST necessárias para entrar em contato com o back-end do aplicativo para se registrar para as notificações por push. Ele também armazena localmente os *registrationIds* criados pelo Hub de Notificação, conforme detalhado em [Registrando-se por meio do back-end do aplicativo](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Ele usa um token de autorização armazenado localmente quando você clica no botão **Fazer logon**.
5. Na sua classe, remova ou comente o campo particular para o `NotificationHub` e adicione um campo para a classe `RegisterClient` e uma cadeia de caracteres para seu ponto de extremidade do back-end ASP.NET. Certifique-se de substituir `<Enter Your Backend Endpoint>` pelo ponto de extremidade de back-end real obtido anteriormente. Por exemplo, `http://mybackend.azurewebsites.net`.

    ```java
    //private NotificationHub hub;
    private RegisterClient registerClient;
    private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
    ```

1. Na sua classe `MainActivity`, no método `onCreate`, remova ou comente a inicialização do campo `hub` e a chamada ao método `registerWithNotificationHubs`. Em seguida, adicione código para inicializar uma instância da classe `RegisterClient` . O método deve conter as linhas a seguir:
   
    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        mainActivity = this;
        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
        gcm = GoogleCloudMessaging.getInstance(this);

        //hub = new NotificationHub(HubName, HubListenConnectionString, this);
        //registerWithNotificationHubs();

        registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

        setContentView(R.layout.activity_main);
    }
    ```
2. Em sua classe `MainActivity`, exclua ou comente o método `registerWithNotificationHubs` inteiro. Ele não será usado neste tutorial.
3. Adicione as seguintes instruções `import` ao seu arquivo **MainActivity.java** .
   
    ```java
    import android.util.Base64;
    import android.view.View;
    import android.widget.EditText;
    
    import android.widget.Button;
    import android.widget.ToggleButton;
    import java.io.UnsupportedEncodingException;
    import android.content.Context;
    import java.util.HashSet;
    import android.widget.Toast;
    import org.apache.http.client.ClientProtocolException;
    import java.io.IOException;
    import org.apache.http.HttpStatus;
    
    import android.os.AsyncTask;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.DefaultHttpClient;
    
    import android.app.AlertDialog;
    import android.content.DialogInterface;
    ```            
4. Substitua o código no método onStart pelo código a seguir: 

    ```java
        super.onStart();
        Button sendPush = (Button) findViewById(R.id.sendbutton);
        sendPush.setEnabled(false);
    ```       
1. Em seguida, adicione os seguintes métodos para manipular o evento de clique do botão **Fazer logon** e enviar notificações por push.
   
    ```java
        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());
   
            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(NotificationSettings.SenderId);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }
   
                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }
   
        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
   
                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;
   
                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");
   
                        HttpResponse response = new DefaultHttpClient().execute(request);
   
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }
   
                    return null;
                }
            }.execute(null, null, null);
        }
    ```

    O manipulador `login` para o botão **Fazer logon** gera um token de autenticação básico usando o nome de usuário e senha de entrada (ele representa qualquer token utilizado pelo esquema de autenticação) e depois usa `RegisterClient` para chamar o back-end para registro.

    O método `sendPush` chama o back-end para disparar uma notificação segura para o usuário com base na marca user. O serviço de notificação de plataforma que `sendPush` tem como destino depende da cadeia de caracteres `pns` passada.

5. Adicione o seguinte método `DialogNotify` à classe `MainActivity`. 

    ```java
        protected void DialogNotify(String title, String message)
        {
            AlertDialog alertDialog = new AlertDialog.Builder(MainActivity.this).create();
            alertDialog.setTitle(title);
            alertDialog.setMessage(message);
            alertDialog.setButton(AlertDialog.BUTTON_NEUTRAL, "OK",
                    new DialogInterface.OnClickListener() {
                        public void onClick(DialogInterface dialog, int which) {
                            dialog.dismiss();
                        }
                    });
            alertDialog.show();
        }
    ```
1. Em sua classe `MainActivity`, atualize o método `sendNotificationButtonOnClick` para chamar o método `sendPush` com aos serviços de notificação de plataforma selecionados do usuário a seguir.
   
    ```java
       /**
        * Send Notification button click handler. This method sends the push notification
        * message to each platform selected.
        *
        * @param v The view
        */
       public void sendNotificationButtonOnClick(View v)
               throws ClientProtocolException, IOException {
   
           String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                   .getText().toString();
           String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                   .getText().toString();
   
           // JSON String
           nhMessage = "\"" + nhMessage + "\"";
   
           if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
           {
               sendPush("wns", nhMessageTag, nhMessage);
           }
           if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
           {
               sendPush("gcm", nhMessageTag, nhMessage);
           }
           if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
           {
               sendPush("apns", nhMessageTag, nhMessage);
           }
       }
    ```
7. No arquivo **build.gradle**, adicione a seguinte linha à seção `android` após a seção `buildTypes`. 

    ```java
    useLibrary 'org.apache.http.legacy'
    ```
8. Compile o projeto. 

## <a name="test-the-app"></a>Testar o aplicativo
1. Execute o aplicativo em um dispositivo ou em um emulador usando o Studio Android.
2. No aplicativo Android, insira um nome de usuário e uma senha. Eles devem ter o mesmo valor de cadeia de caracteres e não devem conter espaços ou caracteres especiais.
3. No aplicativo Android, clique em **Fazer logon**. Aguarde uma mensagem de notificação do sistema que afirma **Conectado e registrado**. Isso habilita o botão **Enviar Notificação**.
   
    ![][A2]
4. Clique nos botões de alternância para habilitar todas as plataformas onde você executou o aplicativo e registrou um usuário.
5. Insira o nome do usuário que recebe a mensagem de notificação. Esse usuário deverá estar registrados para notificações nos dispositivos de destino.
6. Insira uma mensagem para o usuário a ser recebida como uma mensagem de notificação por push.
7. Clique em **Enviar Notificação**.  Cada dispositivo com um registro com a marca username correspondente recebe a notificação de push.

## <a name="next-steps"></a>Próximas etapas
Neste tutorial, você aprendeu como enviar notificações por push para usuários específicos que têm tags associadas seus registros. Para saber como enviar notificações por push, vá para o tutorial a seguir: 

> [!div class="nextstepaction"]
>[Notificações por push com base no local](notification-hubs-push-bing-spartial-data-geofencing-notification.md)


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png

