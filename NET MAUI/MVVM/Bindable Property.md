`BindableProperty` отличается от обычного свойства следующим:
- `BindableProperty` можно использовать в XAML;
- `BindableProperty` может участвовать в привязке.

Синтаксис определения можно просто запомнить. Пример определения ниже.
```csharp
public static readonly BindableProperty FillProperty = BindableProperty.Create(nameof(Fill), typeof(Brush), typeof(GraphViewElement));

public Brush Fill
{
    get => (Brush)GetValue(FillProperty);
    set => SetValue(FillProperty, value);
}
```

Для использования `BindableProperty` необходимо его создать с помощью метода `Create()`. Обязательными параметрами метода являются следующие объекты:
- название свойства, через которое будет доступ к `BindableProperty`, в данном случае это свойство `Fill`;
- тип свойства;
- тип элемента, к которому свойство прикрепляется, зачастую это какой-либо ContentView, в данном случае это `GraphViewElement`.

Также есть и необязательные параметры. Наиболее часто используемые:
- значение по умолчанию;
- метод, который вызывается при изменении свойства (`PropertyChangedCallback()`).

#### Пример с методом `PropertyChangedCallBack`

```csharp
public static readonly BindableProperty TextProperty = BindableProperty.Create(nameof(Text), typeof(string), typeof(Card), 
	propertyChanged: OnTextPropertyChangedCallback);
	
public string Text
{
	get => (string)GetValue(TextProperty);
	set => SetValue(TextProperty, value);
}

private static void OnTextPropertyChangedCallback(BindableObject bindable, object oldValue, object newValue)
{
    var card = (Card)bindable;
	card.Label.Text = newValue as string;
}
```