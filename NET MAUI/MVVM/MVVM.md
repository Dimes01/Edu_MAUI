Фреймворк MAUI предполагает использование паттерна MVVM. Это означает, что приложение должно состоять из 3 компонентов: 
* Model - класс, используемый как модель данных чего-либо;
* View - представление или пользовательский интерфейс;
* ViewModel - класс, который обеспечивает связь Model и View, и реализующий логику приложения.

Преимуществом использования данного паттерна является меньшая связанность между компонентами и разделение ответственности между ними. То есть Model отвечает за данные, View отвечает за графический интерфейс, а ViewModel – за логику приложения.

Для реализации паттерна MVVM в MAUI предусмотрен механизм привязки.

> [!info] Рекомендация
> Не везде следует использовать паттерн MVVM. В таких элементах, как ContentView стоит отдавать предпочтение обработчикам сигналов, нежели механизму привязки. Это связано с тем, что ContentView предполагает написание логики в своём back-code файле и логика его работы не должна быть отделена от представления.

## Примеры реализации Model

#### RouteNode из библиотеки GraphLib 

Маршрутная нода, используемая алгоритмом A-Star для построения маршрута в графе

```csharp
namespace GraphLib.Models;

internal class RouteNode
{
    public RouteNode(Node node, RouteNode parent = null)
    {
        Node = node;
        Parent = parent;
        UpdateFGH();
    }
    public Node Node { get; private set; }
    public Point Position => Node.Position;
    public RouteNode Parent { get; private set; }
    public int F { get; private set; }
    public int G { get; private set; }
    public int H { get; private set; }

    public void UpdateFGH()
    {
        if (Parent == null) return;
        G = Parent.G + Convert.ToInt32(Math.Sqrt(Math.Pow(Parent.Node.Position.X - Node.Position.X, 2) + Math.Pow(Parent.Node.Position.Y - Node.Position.Y, 2)));
        H = Parent.H + Convert.ToInt32(Math.Abs(Route.End.Node.Position.X - Node.Position.X) + Math.Abs(Route.End.Node.Position.Y - Node.Position.Y));
        F = G + H;
    }
}

```

## Примеры реализации View

#### MainPage из Game_OS

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
			 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
			 xmlns:elem="clr-namespace:Game_OS.UIElements"
			 xmlns:local="clr-namespace:Game_OS"
			 x:Class="Game_OS.MainPage"
			 BindingContext="{StaticResource VM_MainPage}">
	<Grid
		BackgroundColor="{AppThemeBinding Light={StaticResource PrimaryColor}, Dark={StaticResource TertiaryColor}}"
		RowDefinitions="Auto,*,Auto">
		<Grid
			Grid.Row="0"
			ColumnDefinitions="*,*" ColumnSpacing="10"
			HorizontalOptions="Center" VerticalOptions="Start">
			<Label 
				Grid.Column="0"
				Text="{Binding CountGames, Mode=OneWay, StringFormat='Число игр: {0:D}'}"
				Style="{StaticResource LabelStyle}"/>
			<Label 
				Grid.Column="1"
				Text="{Binding CountStep, Mode=OneWay, StringFormat='Число ходов: {0:D}'}"
				Style="{StaticResource LabelStyle}"/>
		</Grid>
		<elem:CardPanel
			x:Name="CardPanel"
			Grid.Row="1"
			Items="{Binding AllCards, Mode=OneWay}"/>
		<Grid
			Grid.Row="2"
			ColumnDefinitions="*,*,*,*,*,*">
			<Button
				Grid.Column="0"
				Text="Начать заново"
				Command="{Binding ResetGameCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
			<Button
				Grid.Column="1"
				Text="Влево"
				Command="{Binding LeftCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
			<Button
				Grid.Column="2"
				Text="Вправо"
				Command="{Binding RightCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
			<Button
				Grid.Column="3"
				Text="Сменить"
				Command="{Binding ExchangeCardsCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
			<Button 
				Grid.Column="4"
				Text="+2"
				Command="{Binding AddCardsCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
			<Button 
				Grid.Column="5"
				Text="-2"
				Command="{Binding RemoveCardsCommand, Mode=OneWay}"
				Style="{StaticResource MainButtonStyle}"/>
		</Grid>
	</Grid>
</ContentPage>

```

## Примеры реализации ViewModel

#### MainPageViewModel из Game_OS

ViewModel для главной страницы MainPage из Game_OS
```csharp
using Game_OS.UIElements;
using System.Collections.ObjectModel;
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Text;
using System.Windows.Input;

namespace Game_OS.ViewModels;

class MainPageViewModel : INotifyPropertyChanged
	{
	public event PropertyChangedEventHandler PropertyChanged;
	public void OnPropertyChanged([CallerMemberName] string prop = "") => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(prop));

	public MainPageViewModel()
	{
		GenerateSequence();
		ResetGameCommand.Execute(null);
	}

	private StringBuilder SequenceBuilder { get; set; }
	private string Sequence { get; set; }
	private char SpaceCharacter { get; } = '_';


		private int countCards = 8;
	public int CountCards
	{
		get => countCards;
		private set
		{
			if (countCards == value) return;
			countCards = value;
			OnPropertyChanged(nameof(CountCards));
		}
	}


	private ObservableCollection<Card> allCards = new();
	public ObservableCollection<Card> AllCards
	{
		get => allCards;
		set
		{
			if (allCards == value) return;
			allCards = value;
			OnPropertyChanged(nameof(AllCards));
		}
	}


	private int indexFirstSelectedCard;
	public int IndexFirstSelectedCard
	{
		get => indexFirstSelectedCard;
		private set
		{
			if (indexFirstSelectedCard == value) return;
			indexFirstSelectedCard = value;
			OnPropertyChanged(nameof(IndexFirstSelectedCard));
		}
	}


	private int countStep;
	public int CountStep
	{
		get => countStep;
		set
		{
			if (countStep == value) return;
			countStep = value;
			OnPropertyChanged(nameof(CountStep));
		}
	}


	private int countGames;
	public int CountGames
	{
		get => countGames;
		set
		{
			if (countGames == value) return;
			countGames = value;
			OnPropertyChanged(nameof(CountGames));
		}
	}


	private ICommand addCardsCommand;
	public ICommand AddCardsCommand => addCardsCommand ??= new Command(f =>
	{
		CountCards += 2;
		ResetGameCommand.Execute(null);
	});


	private ICommand removeCardsCommand;
	public ICommand RemoveCardsCommand => removeCardsCommand ??= new Command(f =>
	{
		CountCards -= 2;
		ResetGameCommand.Execute(null);
	});


	private ICommand selectCardCommand;
	public ICommand SelectCardCommand => selectCardCommand ??= new Command(f =>
	{
		if (f is not Card card) return;
		ClearSelected();
		if (AllCards.IndexOf(card) < CountCards - 1 && AllCards.IndexOf(card) >= 0) IndexFirstSelectedCard = AllCards.IndexOf(card);
		var nextCard = AllCards[IndexFirstSelectedCard + 1];
		card.Selected = nextCard.Selected = true;
	});


	private ICommand exchangeCardsCommand;
	public ICommand ExchangeCardsCommand => exchangeCardsCommand ??= new Command(f =>
	{
		if (AllCards[IndexFirstSelectedCard].Text.First() == SpaceCharacter
			|| AllCards[IndexFirstSelectedCard + 1].Text.First() == SpaceCharacter) return;
		var posSpace = Sequence.IndexOf(SpaceCharacter);
		AllCards[posSpace].Text = AllCards[IndexFirstSelectedCard].Text;
		AllCards[posSpace + 1].Text = AllCards[IndexFirstSelectedCard + 1].Text;
		AllCards[IndexFirstSelectedCard].Text = AllCards[IndexFirstSelectedCard + 1].Text = SpaceCharacter.ToString();
		SequenceBuilder[posSpace] = SequenceBuilder[IndexFirstSelectedCard];
		SequenceBuilder[posSpace + 1] = SequenceBuilder[IndexFirstSelectedCard + 1];
		SequenceBuilder[IndexFirstSelectedCard] = SequenceBuilder[IndexFirstSelectedCard + 1] = SpaceCharacter;
		Sequence = SequenceBuilder.ToString();
		++CountStep;
		if (CheckWin())
		{
			++CountGames;
			ResetGameCommand.Execute(null);
		}
	});


	private ICommand resetGameCommand;
	public ICommand ResetGameCommand => resetGameCommand ??= new Command(f =>
	{
		ClearSelected();
		AllCards.Clear();
		GenerateSequence();
		for (int i = 0; i < CountCards; ++i) AddCard(Sequence[i].ToString());
		SelectCardCommand.Execute(AllCards[0]);
		CountStep = 0;
		App.CardPanel?.UpdatePanel();
	});


	private ICommand leftCommand;
	public ICommand LeftCommand => leftCommand ??= new Command(f =>
	{
		if (IndexFirstSelectedCard == 0) return;
		SelectCardCommand.Execute(AllCards[IndexFirstSelectedCard - 1]);
	});


	private ICommand rightCommand;
	public ICommand RightCommand => rightCommand ??= new Command(f =>
	{
		if (IndexFirstSelectedCard >= CountCards - 2) return;
		SelectCardCommand.Execute(AllCards[IndexFirstSelectedCard + 1]);
	});


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

	private void ClearSelected()
	{
		if (AllCards.Count == 0) return;
		AllCards[IndexFirstSelectedCard].Selected = false;
		AllCards[IndexFirstSelectedCard + 1].Selected = false;
	}

	private void GenerateSequence()
	{
		var rnd = new Random();
		do
		{
			Sequence = new('1', CountCards);
			SequenceBuilder = new(Sequence);
			int pos = rnd.Next(CountCards - 1);
			SequenceBuilder[pos] = SequenceBuilder[pos + 1] = SpaceCharacter;
			for (int i = 0; i < (CountCards - 2) / 2; ++i)
			{
				pos = rnd.Next(CountCards);
				if (SequenceBuilder[pos] == SpaceCharacter || SequenceBuilder[pos] == '2') --i;
				else if (SequenceBuilder[pos] == '1') SequenceBuilder[pos] = '2';
			}
			Sequence = SequenceBuilder.ToString();
		} while (CheckWin());
	}

	private bool CheckWin()
	{
		bool flag = false;
		for (int i = 0; i < CountCards; i++)
		{
			if (Sequence[i] == '2') flag = true;
			if (Sequence[i] == '1' && flag) return false;
		}
		return true;
	}
}
```
