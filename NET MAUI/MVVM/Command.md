Команда позволяет View взаимодействовать с ViewModel. Команда похожа на обработчик событий, но отличается тем, что позволяет проектировать ViewModel без необходимости быть тесно связанной с элементами управления.

Команда состоит из 2 методов:
- `Execute()` - логика команды;
- `CanExecute()` - условие, при котором команда может быть выполнена.

Команду можно вызвать как из XAML, так и из C#.
```xml
<Button 
    Text="{Binding Path=CountClick, Mode=OneWay}"
    Command="{Binding Path=AddCountClickCommand, Mode=OneWay}"/>
<!--Команда будет вызвана при нажатии на кнопку-->
```

```csharp
// Команда выполняется при вызове соответсвующего метода с передачей параметра
AddCountClickCommand.Execute(null);
```

#### Примеры реализации команд

```csharp
// 1-ый вариант
public Command AddCountClickCommand => new(f => ++CountClick, f => CountClick < 20);

// 2-ой вариант
private ICommand removeRouteCommand;
public ICommand RemoveRouteCommand => removeRouteCommand ??= new Command(f =>
{
	for (int i = 0; i < elementsOfRoute.Count; ++i) 
		GraphViewElements.Remove(elementsOfRoute[i]);
	elementsOfRoute.Clear();
	NodesForRoute.Clear();
});
```