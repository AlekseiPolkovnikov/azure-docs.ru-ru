1. Щелкните вкладку **Приложения** на странице каталога на [классическом портале Azure](https://manage.windowsazure.com/).
2. Щелкните регистрацию интегрированного приложения.
3. Нажмите кнопку **Настроить** на странице приложения и прокрутите вниз к разделу **Ключи** страницы.
4. Выберите для нового ключа длительность **1 год**. Затем нажмите кнопку **Сохранить**, и на портале отобразится значение нового ключа.
5. Скопируйте **Идентификатор клиента** и **Ключ**, показанные после сохранения. Обратите внимание, что значение ключа будет показано после сохранения только один раз. 
   
    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/client-id-and-key.png)
6. Прокрутите к нижней части страницы настройки встроенного приложения, включите разрешение **Читать данные каталога** для приложения и щелкните кнопку **Сохранить**.
   
    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/app-perms.png)
7. На [классическом портале Azure](https://manage.windowsazure.com/) вернитесь к своей мобильной службе и откройте вкладку **Настройка**. Прокрутите вниз к разделу **Параметры приложения**, добавьте следующие параметры приложения и нажмите кнопку **Сохранить**.
   
    <table border="1"> <tr> <th>Имя параметра приложения</th><th>Описание</th> </tr> <tr> <td>AAD\_CLIENT\_ID</td><td>Идентификатор клиента, скопированный из интегрированного приложения на предыдущих этапах.</td> </tr> <tr> <td>AAD\_CLIENT\_KEY</td><td>Ключ приложения, созданный в интегрированном приложении AAD на предыдущих этапах.</td> </tr> <tr> <td>AAD\_TENANT\_DOMAIN</td><td>Доменное имя AAD. Должно иметь вид mydomain.onmicrosoft.com.</td> </tr> <tr> <td>AAD\_GROUP\_ID</td><td>Идентификатор группы, который был указан для группы "Продажи" в предыдущем разделе.</td> </tr> </table><br/>

    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/aad-app-settings.png)


<!---HONumber=AcomDC_1203_2015-->