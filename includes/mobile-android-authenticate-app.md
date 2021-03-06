
1. В **обозревателе проектов** в Android Studio откройте файл ToDoActivity.java и добавьте следующие операторы import:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
2. Добавьте в класс **ToDoActivity** следующий метод:

        private void authenticate() {
            // Login using the Google provider.

            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();    
                }
            });       
        }

    При этом создается новый метод для обработки процесса проверки подлинности. Пользователь прошел проверку подлинности с использованием имени входа в Google. В диалоговом окне отображается идентификатор пользователя, прошедшего проверку подлинности. Без успешной проверки подлинности продолжение невозможно.

    > [!NOTE]
    > Если вы используете поставщик удостоверений, отличный от Google, измените значение, переданное в методе **login**, выше на одно из следующих: _MicrosoftAccount_, _Facebook_, _Twitter_ или _windowsazureactivedirectory_.

3. Добавьте в метод **onCreate** следующую строку после кода, который формирует экземпляр объекта `MobileServiceClient`.

        authenticate();

    Этот вызов запускает процесс проверки подлинности.
4. Переместите оставшийся код после `authenticate();` в методе **onCreate** в новый метод **createTable**. Он выглядит следующим образом:

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }
5. В меню **Запуск** щелкните **Запуск приложения**, чтобы запустить приложение и выполнить вход с помощью выбранного поставщика удостоверений.

После входа мобильное приложение должно работать без ошибок, а у вас должна быть возможность отправлять запросы в серверную службу и обновлять данные.


<!--HONumber=Dec16_HO2-->


