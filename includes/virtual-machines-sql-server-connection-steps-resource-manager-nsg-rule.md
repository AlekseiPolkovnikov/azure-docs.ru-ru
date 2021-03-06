### Настройка входящего правила группы безопасности сети для виртуальной машины
Если вы хотите иметь возможность подключаться к SQL Server через Интернет, необходимо настроить входящее правило группы безопасности сети для порта, который прослушивает экземпляр SQL Server. По умолчанию это порт TCP 1433.

1. На портале выберите **Виртуальные машины**, а затем виртуальную машину сервера SQL.
2. Затем выберите **Сетевые интерфейсы**.
   
    ![сетевой интерфейс](./media/virtual-machines-sql-server-connection-steps/rm-network-interface.png)
3. Затем выберите сетевой интерфейс виртуальной машины.
4. Щелкните ссылку **Группа безопасности сети**.
   
    ![сетевой интерфейс](./media/virtual-machines-sql-server-connection-steps/rm-network-security-group.png)
5. В свойствах группы безопасности сети разверните **Правила безопасности для входящего трафика**.
6. Нажмите кнопку **Add** (Добавить).
7. В поле **Имя** укажите "SQLServerPublicTraffic".
8. Измените **Протокол** на **TCP**.
9. В поле **Диапазон портов назначения** укажите 1433 (или порт, который прослушивает экземпляр SQL Server).
10. Убедитесь, что в поле **Действие** указано **Разрешить**. Окно настройки правил безопасности должно выглядеть примерно так.
    
     ![правило сетевой безопасности](./media/virtual-machines-sql-server-connection-steps/rm-network-security-rule.png)
11. Нажмите кнопку **ОК**, чтобы сохранить правило для виртуальной машины.

> [!NOTE]
> С подсетью можно связать вторую группу безопасности сети (отличную от группы безопасности сети виртуальной машины). Это не выполняется по умолчанию. Тем не менее, если создать группу безопасности сети для подсети, то необходимо открыть порт 1433 в группе безопасности сети как подсети, так и виртуальной машины.
> 
> 

<!---HONumber=AcomDC_0921_2016-->