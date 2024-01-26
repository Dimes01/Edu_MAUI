Фреймворк MAUI предполагает использование паттерна MVVM. Это означает, что приложение должно состоять из 3 компонентов: 
* Model - класс, используемый как модель данных чего-либо;
* View - представление или пользовательский интерфейс;
* ViewModel - класс, который обеспечивает связь Model и View, и реализующий логику приложения.

Преимуществом использования данного паттерна является меньшая связанность между компонентами и разделение ответственности между ними. То есть Model отвечает за данные, View отвечает за графический интерфейс, а ViewModel – за логику приложения.

Для реализации паттерна MVVM в MAUI предусмотрен механизм привязки.

> [!info] Рекомендация
> Не везде следует использовать паттерн MVVM. В таких элементах, как ContentView стоит отдавать предпочтение обработчикам сигналов, нежели механизму привязки. Это связано с тем, что ContentView предполагает написание логики в своём back-code файле и логика его работы не должна быть отделена от представления.

Более подробный разбор каждого компонента приведён в следующей [заметке](/NET%20MAUI/MVVM/Привязка.md).

## Пример реализации Model

```csharp
class Card
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
}
```

## Примеры реализации View

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:vm="clr-namespace:MauiApp1.ViewModels"
             x:Class="MauiApp1.MainPage"
             Title="MainPage"
             BackgroundColor="AntiqueWhite">
    <ContentPage.BindingContext>
        <vm:MainPageViewModel/>
    </ContentPage.BindingContext>
    <ContentPage.Resources>
        <DataTemplate x:Key="DT_ListItem">
            <ViewCell>
                <ViewCell.View>
                    <StackLayout 
                        BackgroundColor="Azure"
                        Margin="10">
                        <Label 
                            Text="{Binding Id, StringFormat='Id: {0}'}"
                            TextColor="Black"/>
                        <Label 
                            Text="{Binding Name, StringFormat='Name: {0}'}"
                            TextColor="Black"/>
                    </StackLayout>
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ContentPage.Resources>
    <ScrollView>
        <ListView
            ItemsSource="{Binding Cards, Mode=OneWay}"
            ItemTemplate="{DynamicResource DT_ListItem}"/>
    </ScrollView>
</ContentPage>
```

## Примеры реализации ViewModel

```csharp
class MainPageViewModel : INotifyPropertyChanged
{
    private ObservableCollection<Card> cards =
    [
        new Card { Id = 1, Name = "Карта 1" },
        new Card { Id = 2, Name = "Карта 2" },
        new Card { Id = 3, Name = "Карта 3" }
    ];
	public ObservableCollection<Card> Cards
    {
        get => cards;
        set 
        {
            if (cards == value) return;
            cards = value;
            OnPropertyChanged(nameof(Cards));
        }
    }

    public event PropertyChangedEventHandler? PropertyChanged;
    protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null) => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
