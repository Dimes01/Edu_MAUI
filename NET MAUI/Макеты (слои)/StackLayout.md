Макет `StackLayout` располагает элементы в виде стека. Бывают 2 разновидности:
1. `HorizontalStackLayout` - элементы располагаются горизонтально
2. `VerticalStackLayout` - элементы располагаются вертикально

Также, направление расположения можно менять с помощью изменения свойства `Orientation`, которое может применять 2 значения: `Horizontal` и `Vertical`.

```xml
<StackLayout 
	Orientation="Horizontal">
	<!--Равносильно HorizontalStackLayout-->
</StackLayout>
<StackLayout 
	Orientation="Vertical">
	<!--Равносильно VerticalStackLayout-->
</StackLayout>
```
