Данный жест отвечает за нажатие на элементе и реализован классом `TapGestureRecognizer`. У класса есть следующие важные свойства:
- `Buttons` отвечает за то, нажатие какой кнопки вызывает событие:
	- `Primary` - левая кнопка мыши;
	- `Secondary` - правая кнопка мыши;
- `Tapped` - событие нажатия;
- `Command` - команда, выполняющаяся при нажатии;
- `CommandParameter` - параметр команды;
- `NumberOfTapsRequired` - число нажатий, которое требуется для срабатывания жеста; изначально равно 1.

> [!warning]
> По информации на [microsoft_learn](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/gestures/tap?view=net-maui-8.0).
> 
> 1. На Windows нет распознавания по числу нажатий, если нажатий больше 2. 
> 2. Также, на Windows при задании свойству `Buttons` значения `Secondary`, число нажатий может быть равным только 1.

## Получение позиции нажатия
Взято с [microsoft_learn](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/gestures/tap?view=net-maui-8.0#get-the-gesture-position)
```csharp
void OnTapGestureRecognizerTapped(object sender, TappedEventArgs e)
{
    // Position inside window
    Point? windowPosition = e.GetPosition(null);

    // Position relative to an Image
    Point? relativeToImagePosition = e.GetPosition(image);

    // Position relative to the container view
    Point? relativeToContainerPosition = e.GetPosition((View)sender);
}
```

## Пример на XAML
Взято с [microsoft_learn](https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/gestures/tap?view=net-maui-8.0#define-the-button-mask)

```xml
<Image Source="dotnet_bot.png">
	<Image.GestureRecognizers>
		<TapGestureRecognizer 
			Tapped="OnTapGestureRecognizerTapped"
			Buttons="Secondary" />
	</Image.GestureRecognizers> 
</Image>
```

## Пример на C# 
Здесь `Card` - это пользовательский элемент управления ([[ContentView]]), к которому добавляют жест нажатия с командой.

```csharp
private void AddCard(string text)
{
	var card = new Card { Text = text };
	card.GestureRecognizers.Add(new TapGestureRecognizer
	{
		Command = SelectCardCommand,
		CommandParameter = card
	});
	AllCards.Add(card);
}
```