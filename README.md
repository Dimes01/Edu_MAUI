В данном репозитории хранятся заметки, посвящённые MAUI. Для более удобного изучения платформы создан Рекомендуемый порядок изучения, в котором все заметки записаны в том порядке, в котором их рекомендуется прочесть. Также прописаны некоторые рекомендации по написанию кода на MAUI.

> Данный репозиторий рекомендуется склонировать и просматривать через Obsidian или другой просмотрщик md-файлов.

# Рекомендованный порядок изучения

`--` перед названием заметки означает, что заметка ещё не написана

1. Установка и создание проекта
	1. [Установка на Windows](Установка/Установка%20на%20Windows.md)
	2. [Установка на Ubuntu](Установка/Установка%20на%20Ubuntu.md)
2. [Страницы](NET%20MAUI/Страницы/Страницы.md)
	1. [ContentPage](NET%20MAUI/Страницы/ContentPage.md) - наиболее часто используемая страница
	2. [FlyoutPage](NET%20MAUI/Страницы/FlyoutPage.md) - страница с всплывающей частью
	3. `--`[TabbedPage](NET%20MAUI/Страницы/TabbedPage.md) - страница с вкладками
	4. `--`[NavigationPage](NET%20MAUI/Страницы/NavigationPage.md) - страница с возможностями навигации
3. Макеты
	1. [Grid](NET%20MAUI/Макеты%20(слои)/Grid.md) - сетка, используемая почти везде
	2. [StackLayout](NET%20MAUI/Макеты%20(слои)/StackLayout.md) - стек в одном направлении
	3. [AbsoluteLayout](NET%20MAUI/Макеты%20(слои)/AbsoluteLayout.md) - макет с абсолютным расположением элементов
	4. `--`[FlexLayout](NET%20MAUI/Макеты%20(слои)/FlexLayout.md) - макет, похожий на стек, но одновременно по 2 направлениям
	5. [Border](NET%20MAUI/Макеты%20(слои)/Border.md) - макет, позволяющий сделать границу для других макетов
4. [Элементы управления](NET%20MAUI/Элементы%20управления/Элементы%20управления.md)
5. Кастомизация элементов управления
	1. `--`[ControlTemplate](NET%20MAUI/Элементы%20управления/ControlTemplate.md)
	2. `--`[DataTemplate](NET%20MAUI/Элементы%20управления/DataTemplate.md)
	3. `--`[ContentView](NET%20MAUI/Элементы%20управления/ContentView.md) - пользовательский элемент управления
6. Жесты
	1. [Tap](NET%20MAUI/Жесты/Tap.md) - нажатие
	2. `--`[Swipe](NET%20MAUI/Жесты/Swipe.md) - сдвиг
	3. `--`[Pan](NET%20MAUI/Жесты/Pan.md) - панорамирование
	4. `--`[Pinch](NET%20MAUI/Жесты/Pinch.md) - приближение/отдаление
	5. `--`[Pointer](NET%20MAUI/Жесты/Pointer.md)
	6. `--`[Drag and drop](NET%20MAUI/Жесты/Drag%20and%20drop.md) - перемещение чего-либо
7. Паттерн MVVM
	1. [MVVM](NET%20MAUI/MVVM/MVVM.md) - общее представление о паттерне и примеры
	2. [Привязка](NET%20MAUI/MVVM/Привязка.md) - механизм привязки
	3. [Binding](NET%20MAUI/MVVM/Binding.md) - подробнее о привязке
	4. [Bindable Property](NET%20MAUI/MVVM/Bindable%20Property.md) - свойство, используемое для привязки во View
	5. [INotifyPropertyChanged](NET%20MAUI/MVVM/INotifyPropertyChanged.md) - интерфейс, позволяющий создавать свойства для привязки во ViewModel
	6. [Command](NET%20MAUI/MVVM/Command.md) - подробнее о командах
8.  Стили и кастомизация
	1. `--`[Стили](NET%20MAUI/Стили/Стили.md) - общее представление о стилях
	2. `--`[Словари стилей](NET%20MAUI/Стили/Словари%20стилей.md) - словари для хранения стилей
	3. `--`[Триггеры](NET%20MAUI/Стили/Триггеры.md)

## Некоторые рекомендации

1. Если что-то можно сделать через XAML, то лучше сделать это через XAML (естественно в разумных пределах, без сильного усложнения).
2. Использовать паттерн MVVM везде, кроме ContentView.
3. Если используется асинхронный код (`async/await`), то нужно помнить, что доступ к элементам управления осуществляется только из главного потока и для исполнения какой-либо визуальной логики (например изменение TrackBar) нужно использовать методы класса `Dispatcher`.
