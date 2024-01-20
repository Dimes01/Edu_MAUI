`Border` позволяет сделать для других макетов границу. Для задания границы используется свойство `StrokeShape`. Изначально это свойство принимает значение `Rectangle`. В качестве примера сделаем круглую границу для макета `StackLayout`. Для этого в свойство `StrokeShape` передадим объект `RoundRectangle` и зададим свойству `CornerRadius` значение радиуса, равного 10.
```xml
<Border
    BackgroundColor="AliceBlue"
    Margin="10">
    <Border.StrokeShape>
        <RoundRectangle CornerRadius="10"/>
    </Border.StrokeShape>
    <StackLayout>
        <Label 
            Text="1-ый элемент" 
            TextColor="Black"
            HorizontalOptions="Center"/>
        <Label 
            Text="2-ой элемент"
            TextColor="Black"
            HorizontalOptions="Center"/>
    </StackLayout>
</Border>
```

![Пример для Border](Прочее/Скриншоты/Пример%20для%20Border.png)