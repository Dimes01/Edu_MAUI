Контейнер AbsoluteLayout позволяет установить для вложенных элементов точные размеры и координаты расположения на странице ([metanit](https://metanit.com/sharp/maui/3.2.php)). Положение дочерних элементов указывается относительно левого верхнего угла.

## Свойства
Данный макет определяет следующие свойства:
- `LayoutBounds` (тип `Rect`), который определяет границы для размещения дочерних элементов; изначально равно `0,0,AutoSize,AutoSize`;
- `LayoutFlags` (тип `AbsoluteLayoutFlags`), который определяет пропорциональное изменение положения и размера дочерних элементов; изначально никакие пропорциональные изменения не установлены (`AbsoluteLayoutFlags.None`).

## Положение и размер
Положение и размер дочерних элементов можно задать через определение свойств, перечисленных выше. 

```xml
<AbsoluteLayout BackgroundColor="AntiqueWhite">
    <Button 
        Text="Кнопка 1"
        AbsoluteLayout.LayoutBounds="50,50,100,50"/>
    <Button 
        Text="Кнопка 2"
        AbsoluteLayout.LayoutBounds="60,150,120,70"/>
</AbsoluteLayout>
```

Использование `LayoutFlags` хорошо описано на [metanit](https://metanit.com/sharp/maui/3.2.php).