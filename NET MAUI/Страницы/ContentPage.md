ContentPage - страница, которая может хранить 1 макет. Состоит из 2 связанных между собой файлов с расширениями `.xaml` и `.xaml.cs`. 

## Создание в Visual Studio
1. Открыть [[окно добавления элемента]];
2. Выбрать `Установленные / Элементы C# / .NET MAUI / .NET MAUI ContentPage (XAML)`.

## Создание в Visual Studio Code
1. Открыть панель [[добавление нового файла по шаблону]];
2. Выбрать `.NET MAUI ContentPage (XAML)`

## Пример

Стандартная страница содержит следующий код:

#### Файл с расширением `.xaml` 
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MauiApp1.NewPage1"
             Title="NewPage1">
    <VerticalStackLayout>
        <Label
            Text="Welcome to .NET MAUI!"
            VerticalOptions="Center"
            HorizontalOptions="Center" />
    </VerticalStackLayout>
</ContentPage>
```
Здесь можно задать заголовок страницы `Title`.

#### Файл с расширением `.xaml.cs`
```csharp
namespace MauiApp1;

public partial class NewPage1 : ContentPage
{
    public NewPage1()
    {
        InitializeComponent();
    }
}
```
В конструкторе страницы выполняется метод InitializeComponent(), который инициализирует элементы в файле `.xaml`.
