Данный интерфейс позволяет писать свойства во ViewModel, которые умеют уведомлять о своём изменении. Для этого они при изменении вызывают метод `OnPropertyChanged()` который создаёт событие `PropertyChanged`. Реализация метода приведена ниже. В Интернете можно найти множество других, но мы воспользуемся этой.

```csharp
public event PropertyChangedEventHandler? PropertyChanged;

protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = null) => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
```

#### Пример свойства, использующего метод `OnPropertyChanged()`

```csharp
private int countClick;
public int CountClick
{
	get => countClick;
	set
	{
		if (countClick == value) return;
		countClick = value;
		OnPropertyChanged(nameof(CountClick));
	}
}
```

Существует и другая реализация, в которой просто убрано условие.