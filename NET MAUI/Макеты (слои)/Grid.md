Макет `Grid` располагает элементы внутри сетки. Для задания количества строк и столбцов используются свойства `RowDefinitions` и `ColumnDefinitions` соответственно. Доступ к элементам, уже расположенным в сетке, осуществляется через свойство `Children`, которое доступно только для чтения, из-за чего операции добавления, удаления и другие выполняются либо через методы в коде C#, либо через явное задание элементов в XAML.

## Примеры задания строк и столбцов
#### XAML
```xml
<Grid>
	<Grid.RowDefinitions>
		<RowDefinition/> <!--Вся доступная область-->
		<RowDefinition Height="Auto"/> <!--Минимально возможный размер-->
		<RowDefinition Height="40"/> <!--Фиксированный размер-->
	</Grid.RowDefinitions>
	<Grid.ColumnDefinitions>
		<ColumnDefinition Width="*"/> <!--Задание пропорциональных размеров-->
		<ColumnDefinition Width="2*"/> <!--Задание пропорциональных размеров-->
	</Grid.ColumnDefinitions>
	<Label
		Grid.Row="0" Grid.Column="0"
		Text="Hello, World!"/>
	<Label
		Grid.Row="1" Grid.Column="1"
		Text="Hello, World!"/>
	<Button
		Grid.Row="2" Grid.Column="1"
		x:Name="CounterBtn"
		Text="Click me"/>
</Grid>
```

То же самое, но в более компактной форме:
```xml
<Grid
	RowDefinitions="*,Auto,40"
	ColumnDefinitions="*,2*">
	<Label
		Grid.Row="0" Grid.Column="0"
		Text="Hello, World!"/>
	<Label
		Grid.Row="1" Grid.Column="1"
		Text="Hello, World!"/>
	<Button
		Grid.Row="2" Grid.Column="1"
		x:Name="CounterBtn"
		Text="Click me"/>
</Grid>
```

В каждом столбце/строке/ячейке можно использовать другие макеты, а также растягивать их на несколько ячеек (свойства `Grid.ColumnSpan` и `Grid.RowSpan`). Растягивание начинается с той ячейки, которая задана через свойства `Grid.Row` и `Grid.Column`. Также можно задать расстояние между строками и столбцами через свойства `RowSpacing` и `ColumnSpacing` для наглядности элементам будет задан цвет их фона:
```xml
<Grid
	x:Name="SomeGrid"
	RowDefinitions="*,Auto,40" ColumnDefinitions="*,2*"
	RowSpacing="10" ColumnSpacing="5"
	BackgroundColor="Green">
	<Grid
		Grid.Row="0" Grid.Column="2"
		BackgroundColor="Brown">
		<Label
			Grid.Row="0" Grid.Column="0"
			Text="Hello, World!"/>
	</Grid>
	<Grid
		Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
		BackgroundColor="Black">
		<Label
			Grid.Row="1" Grid.Column="1"
			Text="Hello, World!"/>
	</Grid>
	<Grid
		Grid.Row="2" Grid.Column="1"
		BackgroundColor="Black">
		<Label
			Grid.Row="1" Grid.Column="1"
			Text="Hello, World!"/>
	</Grid>
	<Button
		Grid.Row="2" Grid.Column="0"
		x:Name="CounterBtn"
		Text="Click me"
		BackgroundColor="BurlyWood"/>
</Grid>
```
![[Grid Пример сетки.png]]

Для добавления и удаления элемента используются методы `Add()` и `Remove()`. Для задания позиции элементов можно воспользоваться статическими методами класса `Grid`: `Grid.SetRow()` и `Grid.SetColumn()`:
```csharp
// back-code для MainPage.xaml
public MainPage()
{
    InitializeComponent();
    
	// Добавление элемента управления Entry в SomeGrid, определённого в MainPage.xaml
    var entry = new Entry();
    Grid.SetRow(entry, 2);
    Grid.SetColumn(entry, 1);
    SomeGrid.Add(entry);
    SomeGrid.Remove(entry);
}
```

То же самое, но с перегруженным методом `Add()`:
```csharp
// back-code для MainPage.xaml
public MainPage()
{
	InitializeComponent();
	
	// Добавление элемента управления Entry в SomeGrid, определённого в MainPage.xaml
	var entry = new Entry();
	SomeGrid.Add(entry, 1, 2);
	SomeGrid.Remove(entry);
}
```