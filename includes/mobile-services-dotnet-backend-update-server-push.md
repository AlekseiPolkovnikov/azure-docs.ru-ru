
1. В обозревателе решений Visual Studio разверните папку **Контроллеры** в проекте мобильной службы. Откройте файл TodoItemController.cs и обновите определение метода `PostTodoItem` с помощью следующего кода:  
   
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
   
            // Create a WNS native toast.
            WindowsPushMessage message = new WindowsPushMessage();
   
            // Define the XML paylod for a WNS native toast notification 
            // that contains the text of the inserted item.
            message.XmlPayload = @"<?xml version=""1.0"" encoding=""utf-8""?>" +
                                 @"<toast><visual><binding template=""ToastText01"">" +
                                 @"<text id=""1"">" + item.Text + @"</text>" +
                                 @"</binding></visual></toast>";
            try
            {
                var result = await Services.Push.SendAsync(message);
                Services.Log.Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                Services.Log.Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }
   
    Этот код отправляет push-уведомление (с текстом вставленного элемента) после вставки элемента списка дел. В случае возникновения ошибки код добавит запись в журнал ошибок, которую можно просмотреть на вкладке **Журналы** мобильной службы на [классическом портале Azure](https://manage.windowsazure.com/).
   
   > [!NOTE]
   > Вы можете использовать шаблонные уведомления для отправки одного push-уведомления клиентам на нескольких платформах. Дополнительную информацию см. в разделе [Поддержка нескольких платформ устройств из одной мобильной службы](../articles/mobile-services/mobile-services-how-to-use-multiple-clients-single-service.md#push).
   > 
   > 
2. Повторно опубликуйте проект мобильной службы в Azure.

<!---HONumber=AcomDC_1203_2015-->