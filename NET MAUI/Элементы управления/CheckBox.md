## Свойства
`CheckBox` представляет из себя особую кнопку, которая может находиться во включенном состоянии, либо в выключенном. Для этого существует свойство `IsChecked`, которое может принимать значения `true` или `false`. 

Иногда пользователю не должно быть доступно изменение текущего состояния элемента, тогда можно "отключить" его с помощью установки свойству `IsEnabled` значения `false`.

## События
Также у этого элемента есть событие `CheckedChanged`, которое вызывается при изменении свойства `IsChecked`. Пример из [microsoft_learn](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/controls/checkbox?view=net-maui-8.0#respond-to-a-checkbox-changing-state) приведён ниже.

```xml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // sender - объект, вызвавший событие (в данном случае CheckBox)
    // e - объект, содержащий текущее значение CheckBox в свойстве Value
}
```

## Базовое изменение внешнего вида
У данного элемента возможно установить цвет с помощью свойства `Color`.

![CheckBox_Цвета](/Прочее/Скриншоты/CheckBox_Цвета.png)
