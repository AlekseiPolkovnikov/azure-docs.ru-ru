
В предыдущем примере был показан стандартный вход, при котором клиенту нужно подключаться к поставщику удостоверений и службе приложений каждый раз, когда приложение запускается. Мало того что этот метод неэффективен, вы можете столкнуться с проблемами, связанными с использованием приложения, если большое количество клиентов попытаются запустить приложение одновременно. Лучше кэшировать токен авторизации, который возвратила служба приложений, причем делать это до входа через поставщика.

> [!NOTE]
> Токен, выданный службами приложений, можно кэшировать независимо от используемой аутентификации (управляемая службой либо клиентом). Этот учебник использует управляемую сервером проверку подлинности.
> 
> 

1. В файле проекта MainPage.xaml.cs добавьте следующие операторы **using**:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Замените метод **AuthenticateAsync** следующим кодом:
   
        private async System.Threading.Tasks.Task AuthenticateAsync()
        {
            string message;
            // This sample uses the Facebook provider.
            var provider = "Facebook";
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            while (credential == null)
            {
                try
                {
                    // Try to get an existing credential from the vault.
                    credential = vault.FindAllByResource(provider).FirstOrDefault();
                }
                catch (Exception)
                {
                    // When there is no matching resource an error occurs, which we ignore.
                }
   
                if (credential != null)
                {
                    // Create a user from the stored credentials.
                    user = new MobileServiceUser(credential.UserName);
                    credential.RetrievePassword();
                    user.MobileServiceAuthenticationToken = credential.Password;
   
                    // Set the user from the stored credentials.
                    App.MobileService.CurrentUser = user;
   
                    try
                    {
                        // Try to return an item now to determine if the cached credential has expired.
                        await App.MobileService.GetTable<TodoItem>().Take(1).ToListAsync();
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {                        
                        if (ex.Response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
                        {
                            // Remove the credential with the expired token.
                            vault.Remove(credential);
                            credential = null;
                            continue;
                        }
                    }
                }
                else
                {
                    try
                    {
                        // Login with the identity provider.
                        user = await App.MobileService
                            .LoginAsync(provider);                        
   
                        // Create and store the user credentials.
                        credential = new PasswordCredential(provider,
                            user.UserId, user.MobileServiceAuthenticationToken);
                        vault.Add(credential);
                    }
                    catch (MobileServiceInvalidOperationException ex)
                    {
                        message = "You must log in. Login Required";
                    }
                }
                message = string.Format("You are now logged in - {0}", user.UserId);
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
   
    В этой версии **AuthenticateAsync** для доступа к мобильному приложению программа пытается использовать учетные данные, которые хранятся в **PasswordVault**. Для проверки того, что хранимый маркер не просрочен, посылается простой запрос. При возвращении ответа 401 предпринимается обычная попытка входа с использованием поставщика. Обычная попытка входа предпринимается и при отсутствии хранимых учетных данных.
   
   > [!NOTE]
   > Это приложение проверяет завершение срока действия маркеров во время входа, однако срок их действия может истечь и после аутентификации, в процессе использования приложения. Решение по обработке ошибок авторизации, связанных с просроченными токенами, см. в записи [Кэширование и обработка просроченных токенов в SDK под управлением мобильных служб Azure](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).
   > 
   > 
3. Дважды перезапустите приложение.
   
    Обратите внимание, что при первом перезапуске по-прежнему потребуется вход с использованием поставщика. При втором перезапуске будут использоваться кэшированные учетные данные и вход будет пропущен.

<!---HONumber=Oct15_HO3-->