---
title: "Reliable Actors: примечания о сериализации типов субъектов | Документация Майкрософт"
description: "В этой статье приведены сведения о базовых требованиях к определению сериализуемых классов, которые можно использовать для определения интерфейсов и состояний Reliable Actors в Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/19/2016
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 57aec98a681e1cb5d75f910427975c6c3a1728c3
ms.openlocfilehash: f08fc1df10506dead5d049fb2c6cdc29c8f89d90


---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Примечания о сериализации типов надежных субъектов Service Fabric
Аргументы всех методов, типы результатов задач, возвращаемых каждым методом в интерфейсе субъекта, и объекты, хранящиеся в диспетчере состояния субъекта, должны быть [сериализуемыми в контракт данных](https://msdn.microsoft.com/library/ms731923.aspx). Это также относится к аргументам методов, определенных в [интерфейсах событий субъекта](service-fabric-reliable-actors-events.md). (Методы интерфейсов для событий субъектов всегда возвращают значение void.)

## <a name="custom-data-types"></a>Пользовательские типы данных
В этом примере приведенный ниже интерфейс субъекта определяет метод, возвращающий пользовательский тип данных `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Этот интерфейс реализован субъектом, который использует диспетчер состояния для хранения объекта `VoicemailBox` .

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

В этом примере объект `VoicemailBox` сериализуется в следующих случаях.

* Объект передается между экземпляром субъекта и вызывающим объектом.
* Объект сохраняется в диспетчере состояния, где он сохраняется на диск и реплицируются на другие узлы.

Платформа Reliable Actors использует сериализацию DataContract. Поэтому пользовательские объекты данных и их элементы должны быть аннотированы атрибутами **DataContract** и **DataMember**, соответственно.

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Дальнейшие действия
* [Жизненный цикл субъектов и сбор мусора](service-fabric-reliable-actors-lifecycle.md)
* [Таймеры и напоминания субъекта](service-fabric-reliable-actors-timers-reminders.md)
* [События субъекта](service-fabric-reliable-actors-events.md)
* [Повторный вход субъекта](service-fabric-reliable-actors-reentrancy.md)
* [Полиморфизм субъекта и объектно-ориентированные шаблоны проектирования](service-fabric-reliable-actors-polymorphism.md)
* [Диагностика и мониторинг производительности в Reliable Actors](service-fabric-reliable-actors-diagnostics.md)



<!--HONumber=Nov16_HO3-->


