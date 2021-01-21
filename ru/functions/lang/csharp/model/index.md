# Модель программирования на языке C#

Сервис {{ sf-name }} предоставляет две модели программирования на языке C# — с помощью интерфейсов [YcFunction](yc-function.md) и [независимого класса](independent-function.md) с совместимым методом. Обе модели подразумевают написание собственных реализаций интерфейсов, а также дают возможность обеспечить функцию одним аргументом и одним возвращаемым значением любых типов. 

Разница между этими моделями состоит в наличии [контекста вызова](../context.md). Если, например, нужно взаимодействовать с сервисами {{ yandex-cloud }} при помощи [SDK](../sdk.md), то рекомендуем выбрать [YcFunction](yc-function.md).

Вне зависимости от выбранной модели, загрузка проекта осуществляется одним из способов: в виде [опубликованного](https://docs.microsoft.com/ru-ru/dotnet/core/tools/dotnet-publish)  `csproj`-проекта или набора файлов `*cs` с исходным кодом. Последний вариант подходит для обработчиков, не имеющих внешних зависимостей. Также, на класс, содержащий функцию-обработчик, накладываются следующие требования:
1. Он должен быть публичным.
1. У него должен присутствовать публичный конструктор без аргументов. 
    По умолчанию он присутствует. Но если вы создали свой конструктор, принимающий аргументы, нужно также создать конструктор, не принимающий никаких аргументов.
1. Он не должен быть [обобщенным](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/generics/).

Вне засимости от выбранной модели обработчиком может быть [async-метод](https://docs.microsoft.com/ru-ru/dotnet/csharp/programming-guide/concepts/async/), возвращающий `Task` или `Task<T>`. Для варианта с независимым классом дополнительно доступен вариант возвращаемого значения `async void`.

При этом есть два обособленных типа: `byte[]` и `String`, работа с которыми немного отличается от работы с другими типами. При их использовании в качестве типа аргумента функции-обработчика, среда выполнения не будет преобразовывать входящий запрос в `JSON`-интерпретацию для этих типов, а передаст его напрямую в пользовательскую функцию-обработчик. Исключением являются случаи, когда используется параметр [integration=raw](../../../concepts/function-invoke.md#http).