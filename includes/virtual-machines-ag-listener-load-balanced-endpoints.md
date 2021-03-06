Для каждой виртуальной машины с репликой Azure необходимо создать конечную точку с балансировкой нагрузки. Все реплики одного региона должны быть в одной облачной службе и одной виртуальной сети. Чтобы создать реплики группы доступности, охватывающие несколько регионов Azure, необходимо настроить несколько виртуальных сетей. Дополнительные сведения о настройке подключения между виртуальными сетями см. в разделе [Настройка подключения VNet-VNet](../articles/vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. На портале Azure перейдите к каждой виртуальной машине с репликой и просмотрите сведения.
2. Перейдите на вкладку **Конечные точки** для каждой виртуальной машины.
3. Убедитесь, что **Имя** и **Общий порт** необходимой конечной точки прослушивателя еще не используются. В следующем примере используется имя MyEndpoint и порт 1433.
4. На локальном клиенте скачайте и установите [последнюю версию модуля PowerShell](https://azure.microsoft.com/downloads/).
5. Запустите **Azure PowerShell**. Откроется новый сеанс PowerShell с загруженными административными модулями Azure.
6. Выполните командлет **Get-AzurePublishSettingsFile**. Он перенаправит вас в браузер для скачивания файла параметров публикации в локальный каталог. Может потребоваться ввод учетных данных подписки Azure.
7. Выполните команду **Import-AzurePublishSettingsFile**, указав путь к скачанному файлу параметров публикации:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    После импорта файла параметров публикации вы сможете управлять подпиской Azure в сеансе PowerShell.
   
   <!------HONumber=AcomDC_0128_2016-->

