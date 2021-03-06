<!--author=SharS last changed: 01/15/2016-->

#### Чтобы установить обновление 1.2 с классического портала Azure
1. На странице службы StorSimple выберите свое устройство. Перейдите в раздел **Устройства** > **Обслуживание**.
2. В нижней части страницы нажмите кнопку **Поиск обновлений**. Будет создано задание поиска доступных обновлений. Когда оно будет выполнено, вы получите уведомление.
3. В разделе **Обновления программного обеспечения** на той же странице отобразятся новые доступные обновления. Перед установкой обновления 1.2 на устройство рекомендуется ознакомиться с заметками о выпуске.
   
    ![Установка обновлений программного обеспечения](./media/storsimple-install-update-via-portal/InstallUpdate12_11M.png)
4. В нижней части страницы нажмите кнопку **Установить обновления**.
5. После этого введите подтверждение для применения этих исправлений. Нажмите кнопку **ОК**.
6. Откроется диалоговое окно **Установка обновлений**. Устройства должны пройти все проверки, перечисленные в этом диалоговом окне. Эти действия были выполнены до обновления. Выберите вариант **Я понимаю указанное выше требование, и мое устройство готово к обновлению**. Щелкните значок галочки.
   
    ![Сообщение с подтверждением](./media/storsimple-install-update-via-portal/InstallUpdate12_2M.png)
7. Начнется выполнение ряда автоматических предварительных проверок. В частности, описаны такие возможности:
   
   * **Проверки работоспособности контроллера** подтверждают, что оба контроллера устройства готовы к работе и подключены к сети.
   * **Проверки работоспособности компонентов оборудования** подтверждают, что все компоненты оборудования устройства StorSimple готовы к работе.
   * **Проверки DATA 0** подтверждают, что интерфейс DATA 0 включен на устройстве. Если этот интерфейс не включен, включите его и повторите проверку.
   * **Проверки DATA 2 и DATA 3** подтверждают, что сетевые интерфейсы DATA 2 И DATA 3 не включены. Если эти интерфейсы включены, отключите их и попробуйте обновить устройство. Эта проверка выполняется только при обновлении на устройстве с общедоступной версией программного обеспечения. Устройствам, на которых используется версия 0.1, 0.2 или 0.3, эта проверка не требуется.
   * **Проверка шлюза** на любом устройстве, использующем ПО версии до обновления 1. Этой проверке подвергаются все устройства с предварительным обновлением 1; устройства со шлюзом, настроенным для сетевого интерфейса, отличного от DATA 0, пройти эту проверку не могут.
     
     Обновление 1.2 будет установлено только после успешного выполнения всех предварительных проверок. Появится сообщение о том, что выполняется предварительная проверка.
     
     ![Уведомление о предварительной проверке](./media/storsimple-install-update-via-portal/InstallUpdate12_3M.png)
     
     Ниже приведен пример, в котором пройти предварительную проверку не удалось. Необходимо убедиться в том, что оба контроллера устройства находятся в рабочем состоянии и подключены. Также нужно проверить работоспособность аппаратных компонентов. В этом примере необходимо проверить компоненты "Контроллер 0" и "Контроллер 1". Если вы не можете устранить обнаруженные проблемы самостоятельно, обратитесь в службу поддержки Майкрософт.
     
       ![Ошибка при предварительной проверке](./media/storsimple-install-update-via-portal/HCS_PreUpgradeChecksFailed-include.png)
     
     > [!NOTE]
     > После установки обновления 1.2 на устройство StorSimple проверки DATA 2 и DATA 3 и проверка шлюза для последующих обновлений не потребуются. Остальные предварительные проверки останутся необходимыми.
     > 
     > 
8. После успешного завершения предварительной проверки будет создано задание обновления. После его создания вы получите соответствующее уведомление.
   
    ![Создание задания обновления](./media/storsimple-install-update-via-portal/InstallUpdate12_44M.png)
   
    Затем обновление будет установлено на вашем устройстве.
9. Чтобы проконтролировать ход установки, щелкните **Просмотр задания**. На странице **Задания** отображается ход обновления.
   
    ![Ход выполнения задания обновления](./media/storsimple-install-update-via-portal/InstallUpdate12_5M.png)
10. Для установки обновления необходимо несколько часов. Вы можете в любой момент просмотреть сведения о соответствующем задании.
    
    ![Данные задания обновления](./media/storsimple-install-update-via-portal/InstallUpdate12_6M.png)
11. После выполнения задания откройте страницу **Обслуживание** и прокрутите вниз до раздела **Обновления программного обеспечения**.
12. Убедитесь, что на устройстве установлено **обновление 1.2 для StorSimple серии 8000 (6.3.9600.17584)**. Также должна измениться **Дата последнего обновления**.
    
    ![Страница "Обслуживание"](./media/storsimple-install-update-via-portal/InstallUpdate12_10M.png)
13. Вы увидите доступные обновления режима обслуживания. Установка этих обновлений прерывает рабочие процессы устройства и может быть реализована только через интерфейс Windows PowerShell. Чтобы установить эти обновления с помощью Windows PowerShell для StorSimple, выполните указания в статье [Установка обновлений режима обслуживания](../articles/storsimple/storsimple-update-device.md#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!NOTE]
> В некоторых случаях сообщение о наличии обновлений режима обслуживания может отображаться в течение 24 часов после успешной установки обновлений режима обслуживания на устройство.
> 
> 

<!---HONumber=AcomDC_0121_2016-->