<!--author=alkohli last changed: 02/22/2016-->


### <a name="to-configure-and-register-the-device"></a>Настройка и регистрация устройства
1. Доступ к интерфейсу Windows PowerShell через последовательную консоль устройства StorSimple. Инструкции см. в разделе [Использование PuTTY для подключения к последовательной консоли устройства](#use-putty-to-connect-to-the-device-serial-console). **Строго соблюдайте описанный порядок действий, иначе доступ к консоли будет невозможен.**
2. В открывшемся сеансе однократно нажмите клавишу "ВВОД", чтобы вывести командную строку. 
3. Будет предложено выбрать язык устройства. Укажите язык и нажмите клавишу "ВВОД". 
   
    ![Настройка и регистрация StorSimple: устройство 1](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice1-U1-include.png)
4. В открывшемся меню последовательной консоли выберите параметр 1, чтобы войти в систему с полным доступом. 
   
    ![Регистрация StorSimple: устройство 2](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice2_U1-include.png)
   
     Выполните шаги 5–12, чтобы настроить минимально необходимые параметры сети для устройства. **Эти шаги настройки необходимо выполнять на активном контроллере устройства.**  В меню последовательной консоли отображается состояние контроллера в виде сообщения баннера. Если отсутствует подключение к активному контроллеру, разорвите соединение, а затем выполните подключение к активному контроллеру.
5. В командной строке введите свой пароль. Пароль устройства по умолчанию: **Password1**.
6. Введите следующую команду: `Invoke-HcsSetupWizard` 
7. Будет выведен мастер задания сетевых настроек устройства. Введите следующие сведения: 
   
   * IP-адрес для сетевого интерфейса DATA 0
   * Маска подсети
   * Шлюз
   * IP-адрес основного DNS-сервера
     
        Обратите внимание, что система проверяет параметры сети по завершении каждого шага этого процесса.
     
     > [!NOTE]
     > Возможно, необходимо будет подождать несколько минут, пока не будут применены маска подсети и настройки DNS. Если появится сообщение об ошибке "Проверьте сетевое подключение к Data 0", проверьте физическое подключение к сети на сетевом интерфейсе DATA 0 активного контроллера.
     > 
     > 
8. (Необязательно.) Настройте прокси-сервер доступа в Интернет. Хотя использовать веб-прокси не обязательно, **следует знать, что, если в вашей сети имеется веб-прокси, настройку для работы с ним можно выполнить только в этом разделе**. Дополнительные сведения см. в статье [Настройка веб-прокси для устройства StorSimple](../articles/storsimple/storsimple-configure-web-proxy.md).
9. Настройте основной NTP-сервер для своего устройства. NTP-серверы необходимы, так как ваше устройство должно синхронизироваться по времени, чтобы оно могло проверять подлинность с вашими поставщиками облачных служб. Убедитесь, что ваша сеть позволяет передавать NTP-трафик из вашего центра обработки данных в Интернет. Если это невозможно, укажите внутренний NTP-сервер. 
10. По соображениям безопасности срок действия пароля администратора устройства истекает после первого сеанса и его необходимо будет сейчас изменить. При выводе запроса задайте пароль администратора устройства. Требуемая длина пароля администратора устройства от 8 до 15 символов. Пароль должен содержать сочетание из трех следующих типов символов: строчные и прописные буквы, цифры и специальные символы.
    
    <br/>![Регистрация StorSimple: устройство 5](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice5_U1-include.png)
11. На последнем шаге мастер установки выполняет регистрацию устройства в службе "Диспетчер StorSimple". Для этого потребуется ключ регистрации службы, полученный на шаге 2. После ввода ключа регистрации, возможно, необходимо будет подождать 2–3 минуты до регистрации устройства.
    
    > [!NOTE]
    > Чтобы выйти из мастера настройки, можно в любое время нажать CTRL+C. Если вы ввели значения всех сетевых параметров (IP-адрес для Data 0, маску подсети и шлюз), то эти значения будут сохранены.
    > 
    > 
    
    ![Регистрация StorSimple: устройство 6](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice6_U1-include.png)
12. После успешной регистрации устройства отобразится ключ шифрования данных службы. Скопируйте этот ключ и сохраните его в безопасном месте. **Этот ключ вместе с ключом регистрации службы потребуются для регистрации дополнительных устройств в службе диспетчера StorSimple.** См. раздел [Безопасность StorSimple](../articles/storsimple/storsimple-security.md), чтобы ознакомиться с дополнительными сведениями об этом ключе.
    
    ![Регистрация StorSimple: устройство 7](./media/storsimple-configure-and-register-device-u1/HCS_RegisterYourDevice7_U1-include.png)    
    
    > [!NOTE]
    > Чтобы скопировать текст из окна последовательной консоли, просто выделите его. Затем можно будет вставить данные в буфер обмена или в любой текстовый редактор. НЕ используйте сочетание клавиш Ctrl + C для копирования ключа шифрования данных службы. Использование сочетания клавиш CTRL+C приведет к выходу из мастера установки. В результате пароль администратора устройства не будет изменен, и устройство вернется к использованию пароля по умолчанию.
    > 
    > 
13. Выйдите из последовательной консоли.
14. Вернитесь на классический портал Azure и выполните следующие действия:
    
    1. Дважды щелкните службу "Диспетчер StorSimple", чтобы открыть страницу **Быстрый запуск** .
    2. Щелкните **Просмотр подключенных устройств**.
    3. На странице **Устройства** убедитесь, что устройство успешно подключено к службе, посмотрев на его состояние. Состояние устройства должно быть **В сети**.
       
        ![Страница устройств StorSimple](./media/storsimple-configure-and-register-device-u1/HCS_DevicesPageM_U1-include.png) 
       
        Если состояние устройства — **Вне сети**, подождите несколько минут, чтобы устройство могло перейти в оперативный режим. 
       
        Если устройство не подключится к сети через несколько минут, проверьте настройки брандмауэра. Они должны соответствовать [требованиям к сети для устройства StorSimple](../articles/storsimple/storsimple-system-requirements.md). 
       
        Убедитесь, что порт 9354 открыт для исходящего трафика. Этот порт используется служебной шиной для связи устройства и службы диспетчера StorSimple.



<!--HONumber=Nov16_HO2-->


