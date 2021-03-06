---
title: Обзор протокола X12 и пакета интеграции Enterprise | Microsoft Docs
description: Узнайте, как использовать соглашения X12 для создания приложений логики.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: cgronlun

ms.service: app-service-logic
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: deonhe

---
# Интеграция Enterprise с X12
> [!NOTE]
> В этой статье описываются функции X12 службы Logic Apps. Сведения об EDIFACT см. [здесь](app-service-logic-enterprise-integration-edifact.md).
> 
> 

## Создание соглашения X12
Прежде чем начать обмен сообщениями X12, необходимо создать соглашение X12 и сохранить его в учетной записи интеграции. Ниже последовательно описывается создание соглашения X12.

### Вот что понадобится для начала работы
* В подписке Azure должна быть определена [учетная запись интеграции](app-service-logic-enterprise-integration-accounts.md).
* В учетной записи интеграции должны быть определены по крайней мере два [партнера](app-service-logic-enterprise-integration-partners.md).

> [!NOTE]
> При создании соглашения содержимое его файла должно соответствовать типу соглашения.
> 
> 

После [создания учетной записи интеграции](app-service-logic-enterprise-integration-accounts.md) и [добавления партнеров](app-service-logic-enterprise-integration-partners.md) можно создать соглашение X12, выполнив следующие действия.

### На домашней странице портала Azure
Войдите на [портал Azure](http://portal.azure.com "Портал Azure") и сделайте следующее:

1. Выберите **Обзор** в меню слева.

> [!TIP]
> Если вы не видите ссылку **Обзор**, возможно, сначала потребуется развернуть меню. Для этого щелкните ссылку **Показать меню**, расположенную в верхнем левом углу свернутого меню.
> 
> 

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)

1. В поле фильтра поиска введите *интеграция*, а затем в списке результатов выберите **Учетные записи интеграции**. ![](./media/app-service-logic-enterprise-integration-x12/x12-1-3.png)
2. В открывшейся колонке **Учетные записи интеграции** выберите учетную запись интеграции для создания соглашения. Если вы не видите списков учетных записей интеграции, [сначала создайте такую учетную запись](app-service-logic-enterprise-integration-accounts.md "Все об учетных записях интеграции"). ![](./media/app-service-logic-enterprise-integration-x12/x12-1-4.png)
3. Выберите плитку **Соглашения**. Если вы не видите эту плитку, добавьте соглашение. ![](./media/app-service-logic-enterprise-integration-x12/x12-1-5.png)
4. В открывшейся колонке "Соглашения" щелкните **Добавить**. ![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)
5. В открывшейся колонке "Соглашения" введите **имя** соглашения, затем задайте значения параметров **Agreement type** (Тип соглашения), **Host Partner** (Главный партнер), **Host Identity** (Идентификатор главного партнера), **Guest Partner** (Партнер-гость), **Guest Identity** (Идентификатор гостя). ![](./media/app-service-logic-enterprise-integration-x12/x12-1.png)
6. Настроив параметры получения, нажмите кнопку **ОК**. Продолжим настройку.
7. Выберите **Receive Settings** (Параметры получения), чтобы настроить обработку сообщений, получаемых посредством данного соглашения.
8. Элемент управления "Receive Settings" (Параметры получения) состоит из приведенных ниже разделов, в том числе "Identifiers" (Идентификаторы), "Acknowledgment" (Подтверждение), "Schemas" (Схемы), "Envelopes" (Оболочка), "Control Numbers" (Контрольные номера), "Validations" (Проверки) и "Internal Settings" (Внутренние параметры). Настройте доступные в них свойства в соответствии с вашим соглашением с партнером, с которым вы будете обмениваться сообщениями. Ниже приведено представление этих элементов управления. Настройте их в зависимости от того, как посредством соглашения должны идентифицироваться и обрабатываться входящие сообщения. ![](./media/app-service-logic-enterprise-integration-x12/x12-2.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-3.png)

1. Нажмите кнопку **ОК**, чтобы сохранить параметры.

### "Identifiers" (Идентификаторы)
| Свойство | Описание |
| --- | --- |
| ISA1 (Authorization Qualifier) (ISA1 (квалификатор авторизации)) |Выберите значение квалификатора авторизации из раскрывающегося списка. |
| ISA2 |необязательный параметр. Введите сведения об авторизации. Если указанное для ISA1 значение отличается от 00, то введите от одной до десяти букв или цифр. |
| ISA3 (Security Qualifier) (ISA3 (квалификатор безопасности)) |Выберите значение квалификатора безопасности из раскрывающегося списка. |
| ISA4 |необязательный параметр. Введите сведения о безопасности. Если указанное для ISA3 значение отличается от 00, то введите от одной до десяти букв или цифр. |

### Благодарности
| Свойство | Описание |
| --- | --- |
| TA1 expected (Ожидается TA1) |Установите этот флажок, чтобы возвращать техническое подтверждение (TA1) отправителю при обмене. Эти подтверждения отсылаются отправителю при обмене в зависимости от параметров отправки для соглашения. |
| FA expected (Ожидается функциональное подтверждение) |Установите этот флажок, чтобы возвращать функциональное подтверждение (FA) отправителю при обмене. Затем согласно версиям схемы, с которыми вы работаете, укажите, требуются ли подтверждения 997 или 999. Эти подтверждения отсылаются отправителю при обмене в зависимости от параметров отправки для соглашения. |
| Include AK2/IK2 Loop (Включить цикл AK2/IK2) |Установите этот флажок, чтобы включить генерирование циклов AK2 в функциональных подтверждениях для принимаемых наборов транзакций. Примечание. Этот флажок становится доступен только в том случае, если установлен флажок "FA expected" (Ожидается функциональное подтверждение). |

### Схемы
Выберите схему для каждого типа транзакции (ST1) и приложения отправителя (GS2). Конвейер получения будет разбирать входящие сообщения, сопоставляя значения ST1 и GS2 во входящем сообщении с заданными здесь значениями, а схему входящего сообщения — с указанной здесь схемой.

| Свойство | Описание |
| --- | --- |
| Version (версия) |Выберите версию X12. |
| Transaction Type (ST01) (Тип транзакции (ST01)) |Выберите тип транзакции. |
| Sender Application (GS02) (Приложение отправителя (GS02)) |Выберите приложение отправителя. |
| Схема |Выберите файл схемы для использования. Файлы схемы находятся в вашей учетной записи интеграции. |

### Envelopes (Оболочка)
| Свойство | Описание |
| --- | --- |
| ISA11 Usage (Использование ISA11) |Это поле используется для указания разделителя в наборе транзакций.</br></br>Выберите стандартный идентификатор, чтобы использовать десятичное представление "." вместо десятичного представления входящего документа в конвейере получения EDI.</br></br>Выберите разделитель повторений, чтобы указать разделитель для повторяющихся вхождений простого элемента данных или повторяющейся структуры данных. Например, в качестве разделителя повторений обычно используется (^). Для схем HIPPA можно использовать только (^). |

### Control Numbers (Контрольные номера)
| Свойство | Описание |
| --- | --- |
| Disallow Interchange Control Number duplicates (Запретить повторяющиеся контрольные номера обмена) |Установите этот флажок, чтобы запретить повторяющиеся операции обмена. Если флажок установлен, то портал служб BizTalk проверяет, соответствует ли контрольный номер (ISA13) полученного обмена контрольному номеру обмена. Если они совпадают, то конвейер получения не обрабатывает обмен.<br/>Если вы решили запретить повторяющиеся контрольные номера, можно указать число дней, в течение которых выполняется проверка, задав соответствующее значение "Check for duplicate ISA13 every x days" (Проверять наличие повторов ISA13 каждые x дн.). |
| Disallow Group control number duplicates (Запретить повторяющиеся контрольные номера групп) |Установите этот флажок, чтобы запретить операции обмена с повторяющимися контрольными номерами групп. |
| Disallow Transaction set control number duplicates (Запретить повторяющиеся контрольные номера наборов транзакций) |Установите этот флажок, чтобы запретить операции обмена с повторяющимися контрольными номерами наборов транзакций. |

### Validations (Проверки)
| Свойство | Описание |
| --- | --- |
| Message Type (Тип сообщения) |Тип сообщения EDI, например 850 (заказ на покупку) или 999 (подтверждение реализации). |
| EDI Validation (Проверка EDI) |Выполняет проверку EDI для типов данных в соответствии со свойствами EDI схемы, ограничениями длины, пустыми элементами данных и конечными разделителями. |
| Extended Validation (Расширенная проверка) |Если тип данных не EDI, то проверка проводится согласно требованиям к элементу данных и разрешены повторы, перечисления и контроль длины элементов данных (минимальная и максимальная длина). |
| Allow Leading/Trailing Zeroes (Разрешить начальные и конечные нули) |Сохраняются все дополнительные начальные или конечные пробелы либо знаки нуля. Они не удаляются. |
| Trailing Separator Policy (Политика конечных разделителей) |Создает конечные разделители в полученном обмене. Возможные значения: "NotAllowed" (Запрещено), "Optional" (Необязательно) и "Mandatory" (Обязательно). |

### Internal Settings (Внутренние параметры)
| Свойство | Описание |
| --- | --- |
| Convert implied decimal format Nn to base 10 numeric value (Преобразовать подразумеваемый десятичный формат Nn в числовое значение с основанием 10) |Преобразует число EDI, которое указано в формате Nn, в числовое значение с основанием 10 в промежуточном коде XML на портале служб BizTalk. |
| Create empty XML tags if trailing separators are allowed (Создать пустые теги XML, если конечные разделители разрешены) |Установите этот флажок, чтобы отправитель при обмене добавлял пустые теги XML для конечных разделителей. |
| Inbound batching processing (Пакетная обработка входящего трафика) |"Split Interchange as transaction sets - suspend transaction sets on error" (Разделять обмен на наборы транзакций: приостанавливать наборы транзакций при ошибке): позволяет анализировать каждый набор транзакций в операции обмена в виде отдельного документа XML, применяя соответствующую оболочку к набору транзакций. Если этот параметр включен, то в случае, если один или несколько наборов транзакций в операции обмена не проходят проверку, службы BizTalk приостанавливают обработку только этих наборов. </br></br>"Разделение документа Interchange на наборы транзакций — заблокировать документ Interchange при ошибке": позволяет анализировать каждый набор транзакций в операции обмена в виде отдельного XML-документа, применяя соответствующую оболочку. Если этот параметр включен, то в случае, если один или несколько наборов транзакций в операции обмена не проходят проверку, службы BizTalk приостанавливают весь обмен.</br></br>"Preserve Interchange - suspend transaction sets on error" (Сохранить обмен: приостанавливать обработку наборов транзакций в случае ошибки): позволяет не затрагивать операцию обмена, создавая документ XML для всего пакетного обмена. Если этот параметр включен, то в случае, если один или несколько наборов транзакций в операции обмена не проходят проверку, службы BizTalk приостанавливают обработку только этих наборов транзакций и продолжают обработку всех прочих наборов транзакций.</br></br>"Preserve Interchange - suspend interchange on error" (Сохранить обмен: приостанавливать обмен в случае ошибки): позволяет не затрагивать операцию обмена, создавая документ XML для всего пакетного обмена. Если этот параметр включен, то в случае, если один или несколько наборов транзакций в операции обмена не проходят проверку, службы BizTalk приостанавливают весь обмен.</br></br> |

Ваше соглашение готово к обработке входящих сообщений, которые соответствуют выбранной схеме.

Для настройки параметров обработки сообщений, отправляемых партнерам, выполните следующее.

1. Выберите **Настройки отправки**, чтобы настроить обработку сообщений, отправляемых при помощи данного соглашения.

Элемент управления "Send Settings" (Параметры отправки) состоит из приведенных ниже разделов, в том числе "Identifiers" (Идентификаторы), "Acknowledgment" (Подтверждение), "Schemas" (Схемы), "Envelopes" (Оболочка), "Control Numbers" (Контрольные номера), "Character Sets and Separators" (Наборы символов и разделители) и "Validation" (Проверка).

Ниже приведено представление этих элементов управления. Выберите параметры в соответствии с тем, как требуется обрабатывать сообщения, отправляемые партнерам посредством этого соглашения. ![](./media/app-service-logic-enterprise-integration-x12/x12-4.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-5.png)

![](./media/app-service-logic-enterprise-integration-x12/x12-6.png)

1. Нажмите кнопку **ОК**, чтобы сохранить параметры.

### "Identifiers" (Идентификаторы)
| Свойство | Описание |
| --- | --- |
| Authorization qualifier (ISA1) (Квалификатор авторизации (ISA1)) |Выберите значение квалификатора авторизации из раскрывающегося списка. |
| ISA2 |Введите сведения об авторизации. Если значение отличается от 00, то введите от одной до десяти букв или цифр. |
| Security qualifier (ISA3) (Квалификатор авторизации (ISA3)) |Выберите значение квалификатора безопасности из раскрывающегося списка. |
| ISA4 |Введите сведения о безопасности. Если значение в текстовом поле "ISA4" отличается от 00, то введите от одной до десяти букв или цифр. |

### Acknowledgment (Подтверждение)
| Свойство | Описание |
| --- | --- |
| TA1 expected (Ожидается TA1) |Установите этот флажок, чтобы возвращать техническое подтверждение (TA1) отправителю при обмене. Этот параметр указывает, что главный партнер, отправляющий сообщение, запрашивает подтверждение от партнера-гостя в соглашении. Эти подтверждения ожидаются главным партнером согласно параметрам получения в соглашении. |
| FA expected (Ожидается функциональное подтверждение) |Установите этот флажок, чтобы возвращать функциональное подтверждение (FA) отправителю при обмене. Затем согласно версиям схемы, с которыми вы работаете, укажите, требуются ли подтверждения 997 или 999. Эти подтверждения ожидаются главным партнером согласно параметрам получения в соглашении. |
| FA Version (Версия функционального подтверждения) |Выберите версию функционального подтверждения. |

### Схемы
| Свойство | Описание |
| --- | --- |
| Version (версия) |Выберите версию X12. |
| Transaction Type (ST01) (Тип транзакции (ST01)) |Выберите тип транзакции. |
| SCHEMA (Схема) |Выберите схему для использования. Схемы находятся в вашей учетной записи интеграции. Для получения доступа к схемам следует сначала связать учетную запись интеграции с приложением логики. |

### Envelopes (Оболочка)
| Свойство | Описание |
| --- | --- |
| ISA11 Usage (Использование ISA11) |Это поле используется для указания разделителя в наборе транзакций.</br></br>Выберите стандартный идентификатор, чтобы использовать десятичное представление "." вместо десятичного представления входящего документа в конвейере получения EDI.</br></br>Выберите разделитель повторений, чтобы указать разделитель для повторяющихся вхождений простого элемента данных или повторяющейся структуры данных. Например, в качестве разделителя повторений обычно используется (^). Для схем HIPPA можно использовать только (^).</br> |
| Repetition separator (Разделитель повторений) |Введите разделитель повторений. |
| Control Version Number (ISA12) (Контрольный номер версии (ISA12)) |Выберите версию стандарта X12, используемую для создания исходящего обмена порталом служб BizTalk. |
| Usage Indicator (ISA15) (Индикатор использования (ISA15)) |Укажите контекст обмена: информация (I), производственные данные (P) или тестовые данные (T). Конвейер получения EDI передает это свойство в контекст. |
| Схема |Можно настроить то, как портал служб BizTalk создает сегменты GS и ST для обмена с кодированием X12, которые отправляются в конвейер отправки.</br></br>Вы можете связать значения элементов данных GS1, GS2, GS3, GS4, GS5, GS7 и GS8 со значениями типа транзакции, а также элементов данных версии и выпуска. Когда портал служб BizTalk определяет, что сообщение XML содержит в строке сетки значения типа транзакции и элементов данных версии и выпуска, он заполняет элементы данных GS1, GS2, GS3, GS4, GS5, GS7 и GS8 в оболочке исходящего обмена значениями из этой же строки сетки. Значения типа транзакции и элементов данных версии и выпуска должны быть уникальными.</br></br>Необязательный параметр. Для GS1 выберите значение функционального кода в раскрывающемся списке.</br></br>Обязательный параметр. Для GS2 введите значение отправителя приложения, содержащее от 2 до 15 букв или цифр.</br></br>Обязательный параметр. Для GS3 введите значение получателя приложения, содержащее от 2 до 15 букв или цифр.</br></br>Необязательный параметр. Для GS4 выберите CCYYMMDD или YYMMDD.</br></br>Необязательный параметр. Для GS5 выберите HHMM, HHMMSS или HHMMSSdd.</br></br>Необязательный параметр. Для GS7 выберите значение ответственного агентства из раскрывающегося списка.</br></br>Необязательный параметр. Для GS8 введите значение для идентификации документа, содержащее от 2 до 12 букв или цифр.</br></br>**Примечание**. Это значения, которые портал служб BizTalk вводит в поля GS создаваемого обмена, если значения типа транзакции и элементов данных версии и выпуска в той же строке совпадают со значениями, связанными с обменом. |

### Control Numbers (Контрольные номера)
| Свойство | Описание |
| --- | --- |
| Interchange Control Number (ISA13) (Контрольный номер обмена (ISA13)) |обязательный параметр. Введите диапазон значений контрольного номера обмена, используемых порталом служб BizTalk при создании исходящего обмена. Введите числовое значение от 1 до 999 999 999. |
| Group Control Number (GS06) (Контрольный номер группы (GS06)) |обязательный параметр. Введите диапазон чисел, которые порталу служб BizTalk следует использовать в качестве контрольного номера группы. Введите числовое значение, содержащее от 1 до 9 разрядов. |
| Transaction Set Control Number (ST02) (Контрольный номер набора транзакций (ST02)) |Введите диапазон числовых значений в обязательные поля посередине и буквы или цифры в необязательные поля префикса и суффикса. Максимальная длина всех четырех полей составляет 9 знаков. |
| Prefix (Префикс) |Чтобы указать диапазон контрольных номеров наборов транзакций, используемых в подтверждении, введите значения в поля "ACK Control number (ST02)" (Контрольный номер подтверждения (ST02)). Введите числовое значение в два поля посередине и буквы или цифры (при необходимости) в поля префикса и суффикса. Поля посередине являются обязательными и содержат минимальное и максимальное значения контрольного номера. Поля префикса и суффикса являются необязательными. Максимальная длина всех трех полей составляет 9 знаков. |
| Суффикс |Чтобы указать диапазон контрольных номеров наборов транзакций, используемых в подтверждении, введите значения в поля "ACK Control number (ST02)" (Контрольный номер подтверждения (ST02)). Введите числовое значение в два поля посередине и буквы или цифры (при необходимости) в поля префикса и суффикса. Поля посередине являются обязательными и содержат минимальное и максимальное значения контрольного номера. Поля префикса и суффикса являются необязательными. Максимальная длина всех трех полей составляет 9 знаков. |

### Character Sets and Separators (Наборы символов и разделители)
Отдельно от набора символов можно ввести отличающийся набор разделителей для каждого типа сообщений. Если для заданной схемы сообщений набор символов не указан, то используется набор символов по умолчанию.

| Свойство | Описание |
| --- | --- |
| Character Set to be used (Используемый набор символов) |Выберите набор символов X12 для проверки свойств, задаваемых для соглашения.</br></br>**Примечание**. Портал служб BizTalk использует этот параметр только для проверки значений, введенных для связанных свойств соглашения. Конвейер получения или отправки игнорирует это свойство набора символов при обработке во время выполнения. |
| Схема |Символ щелкните знак плюс (+) и выберите схему из раскрывающегося списка. Выберите разделители, которые будут использоваться для выбранной схемы:</br></br>Component element separator (Разделитель составляющих элементов) — введите один знак для разделения составных элементов данных.</br></br>Data Element Separator (Разделитель элементов данных) — введите один знак для разделения простых элементов данных в составных элементах данных.</br></br></br></br>Replacement Character (Знак замены) — установите этот флажок, если полезные данные содержат знаки, которые используются также как разделители данных, сегментов или составляющих элементов. После этого вы сможете ввести знак для замены. При создании исходящего сообщения X12 все экземпляры знаков разделителей в полезных данных будут заменены указанным знаком.</br></br>"Segment Terminator" (Признак конца сегмента) — введите знак для указания конца сегмента EDI.</br></br>"Suffix" (Суффикс) — выберите знак, который будет использоваться с идентификатором сегмента. Если указывается суффикс, то элемент данных признака конца сегмента может быть пустым. Если признак конца сегмента оставить пустым, то необходимо будет указать суффикс. |

### Проверка
| Свойство | Описание |
| --- | --- |
| Message Type (Тип сообщения) |Этот параметр позволяет включить проверку для получателя обмена. Таким образом вы сможете выполнять проверку EDI для элементов данных набора транзакций, включая типы данных, ограничения длины, пустые элементы данных и конечные разделители. |
| EDI Validation (Проверка EDI) | |
| Extended Validation (Расширенная проверка) |Этот параметр позволяет включить расширенную проверку обмена, получаемого от отправителя при обмене. В дополнение к проверке типов данных XSD она включает в себя проверку длины поля, необязательности и числа повторов. Можно включить расширенную проверку, не включая проверку EDI, и наоборот. |
| Allow Leading/Trailing Zeroes (Разрешить начальные и конечные нули) |Если выбрать этот параметр, то обмен EDI, полученный от какой-либо стороны, пройдет проверку, если элемент данных в обмене EDI не соответствует требованиям к длине из-за конечных пробелов, но соответствует этим требованиям после их удаления. |
| Trailing Separator (Конечный разделитель) |Если выбрать этот параметр, то обмен EDI, полученный от какой-либо стороны, пройдет проверку, если элемент данных в обмене EDI не соответствует требованиям к длине из-за начальных (или конечных) нулей или конечных пробелов, но соответствует этим требованиям после их удаления.</br></br>Выберите значение "Запрещено", если хотите запретить конечные разделители в обмене, получаемом от отправителя. Если обмен содержит конечные разделители, он будет объявлен недопустимым.</br></br>Выберите значение "Необязательно", чтобы принимать обмен с конечными разделителями или без них.</br></br>Выберите значение "Обязательно", если получаемый обмен должен содержать конечные разделители. |

После нажатия кнопки **ОК** откроется колонка.

1. Выберите элемент **Соглашения** в колонке "Учетная запись интеграции", и вы увидите новое соглашение в списке. ![](./media/app-service-logic-enterprise-integration-x12/x12-7.png)

## Подробнее
* [Узнайте больше о пакете интеграции Enterprise.](app-service-logic-enterprise-integration-overview.md "Узнайте о пакете интеграции Enterprise.")

<!---HONumber=AcomDC_0803_2016-->