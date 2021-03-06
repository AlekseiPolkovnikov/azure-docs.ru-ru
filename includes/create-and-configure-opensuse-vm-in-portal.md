---
writer: cynthn
editor: tysonn
manager: timlt

{}

---
1. Войдите на [классический портал Azure](http://manage.windowsazure.com).  
2. В панели команд внизу окна нажмите **Создать**.
3. В разделе **Среда выполнения приложений** нажмите **Виртуальная машина**, а затем **Из коллекции**.
   
    ![Создание новой виртуальной машины][Image1]
4. В группе **SUSE** выберите образ виртуальной машины OpenSUSE, а затем щелкните стрелку для продолжения.
5. На странице **Конфигурация виртуальной машины** выполните следующие действия:
   
   * Введите имя в поле **Имя виртуальной машины**, например "testlinuxvm". Имя должно содержать от 3 до 15 символов, может включать только буквы, цифры и дефисы, а также должно начинаться с буквы и заканчиваться буквой или цифрой.
   * Проверьте значение параметра **Уровень** и выберите значение параметра **Размер**. Размеры, доступные для выбора, зависят от указанного уровня. Размер определяет расходы на виртуальную машину, а также возможности настройки, например число подключаемых дисков с данными. Дополнительную информацию см. в статье [Размеры виртуальных машин](../articles/virtual-machines/virtual-machines-linux-sizes.md).
   * Введите значение в поле **Имя нового пользователя** или примите имя по умолчанию — **azureuser**. Это имя добавляется в файл списка Sudoers.
   * Выберите тип проверки подлинности и задайте соответствующее значение для параметра **Проверка подлинности**. Общие рекомендации по созданию пароля см. в статье [Надежные пароли](http://msdn.microsoft.com/library/ms161962.aspx).
6. На следующей странице **Конфигурация виртуальной машины** выполните следующие действия:
   
   * Используйте значение по умолчанию параметра **Создать новую облачную службу**.
   * В поле **DNS-имя** введите уникальное DNS-имя, которое будет использоваться в качестве части адреса, например testlinuxvm.
   * — В поле **Регион, территориальная группа, виртуальная сеть** выберите регион, где будет размещаться этот виртуальный образ.
   * В разделе **Конечные точки** оставьте конечную точку SSH. Вы можете добавить другие точки прямо сейчас либо добавить, изменить или удалить их после создания виртуальной машины.
     
     > [!NOTE]
     > Если необходимо, чтобы виртуальная машина использовала виртуальную сеть, то при создании виртуальной машины **обязательно** укажите виртуальную сеть. Вы не сможете добавить виртуальную машину в виртуальную сеть после создания ВМ. Дополнительные сведения см. в статье [Общие сведения о виртуальных сетях](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. На последней странице **Конфигурация виртуальной машины** сохраните параметры по умолчанию и щелкните значок с галочкой для завершения.

Виртуальная машина отображается на портале в разделе **Виртуальные машины**. Пока для состояния отображается значение **(Подготовка)**, осуществляется настройка виртуальной машины. Когда состояние меняется на **Работает**, можно переходить к следующему шагу.

## Подключение к виртуальной машине
В зависимости от операционной системы того компьютера, с которого осуществляется подключение, к виртуальной машине можно подключиться с помощью SSH или PuTTY:

* На компьютере под управлением Linux используйте SSH. В командной строке выполните следующую команду:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Введите пароль пользователя.
* На компьютере под управлением Windows используйте PuTTY. Если он не установлен, скачайте его на [странице скачивания PuTTY][PuTTYDownload].
  
    Сохраните файл **putty.exe** в каталог на своем компьютере. Откройте окно командной строки, перейдите к этой папке и запустите **putty.exe**.
  
    Введите имя узла, например "testlinuxvm.cloudapp.net", и введите в поле **Порт** значение "22".
  
    ![Экран PuTTY][Image6]

## Обновление виртуальной машины (необязательно)
1. После подключения к виртуальной машине можно, при необходимости, установить системные обновления и исправления. Чтобы запустить обновления, введите:
   
    `$ sudo zypper update`
2. Выберите **Программное обеспечение**, а затем **Обновление по сети**, чтобы открыть список доступных обновлений. Нажмите кнопку **Принять**, чтобы запустить установку и применить все новые исправления (кроме дополнительных), доступные для системы.
3. После завершения установки нажмите **Готово**. Теперь система обновлена.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image1]: ./media/create-and-configure-opensuse-vm-in-portal/CreateVM.png

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png

