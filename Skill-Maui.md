# Skill - .NET MAUI

Mejores prácticas y convenciones para proyectos .NET, con especialización en .NET MAUI.

---

## Reglas globales

### Comentarios en inglés

Todos los comentarios del código deben escribirse **siempre en inglés**, independientemente del tipo de archivo o del idioma del proyecto.

---

## Archivos XAML

### 1. Eliminar la declaración XML

La línea de declaración XML **siempre debe ser eliminada** de los archivos `.xaml`:

```xml
<!-- REMOVE this line -->
<?xml version="1.0" encoding="utf-8" ?>
```

### 2. Orden de propiedades en el elemento raíz

Las propiedades del elemento raíz deben organizarse en **dos bloques ordenados alfabéticamente**, en este orden:

1. **Bloque `xmlns:`** — Espacios de nombres. Ordenados alfabéticamente entre sí.
2. **Bloque `x:`** — Propiedades del namespace `x:`. Ordenadas alfabéticamente entre sí.

Cada bloque se trata como independiente para la ordenación.

```xml
<!-- Correct -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:local="clr-namespace:MyApp"
    xmlns:viewmodels="clr-namespace:MyApp.ViewModels"
    x:Class="MyApp.MainPage"
    x:Name="Root">

<!-- Incorrect (unordered, x: mixed with xmlns:) -->
<ContentPage
    xmlns:viewmodels="clr-namespace:MyApp.ViewModels"
    x:Class="MyApp.MainPage"
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:local="clr-namespace:MyApp"
    x:Name="Root">
```

### 3. Eliminar espacios de nombres no utilizados

Toda declaración `xmlns:` que no se esté utilizando en el cuerpo del archivo XAML **debe ser eliminada**.

```xml
<!-- If "toolkit" is not used anywhere in the XAML, remove its declaration -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"
    x:Class="MyApp.MainPage">

<!-- Only declare what is used -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    x:Class="MyApp.MainPage">
```

### 4. Separación entre elementos con líneas en blanco

Cada elemento hijo debe tener **una línea en blanco antes y después** para mejorar la legibilidad. La excepción son las **propiedades adjuntas** del elemento padre (como `Grid.ColumnDefinitions`, `Grid.RowDefinitions`, etc.), que van pegadas directamente a su elemento sin línea en blanco de por medio.

```xml
<!-- Correct -->
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <controls:ImageControl />

    <controls:LabelControl Grid.Column="1" />

</Grid>

<!-- Incorrect (no separation between elements) -->
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>
    <controls:ImageControl />
    <controls:LabelControl Grid.Column="1" />
</Grid>

<!-- Incorrect (blank line between element and its attached properties) -->
<Grid>

    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <controls:ImageControl />

    <controls:LabelControl Grid.Column="1" />

</Grid>
```

#### Excepciones

**Span** — Los elementos `Span` dentro de un `FormattedString` van sin líneas en blanco, ya que forman parte de un mismo bloque de texto:

```xml
<!-- Correct -->
<controls:LabelControl>
    <controls:LabelControl.FormattedText>
        <FormattedString>
            <controls:SpanControl />
            <controls:SpanControl />
        </FormattedString>
    </controls:LabelControl.FormattedText>
</controls:LabelControl>
```

**DataTemplate** — El contenido de un `DataTemplate` va compacto, sin líneas en blanco entre el template y su contenido:

```xml
<!-- Correct -->
<VerticalStackLayout>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <cells:Cell />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</VerticalStackLayout>
```

### 5. Espacio antes del cierre de elementos autocerrados

Los cierres de elementos autocerrados (`/>`) deben ir **siempre precedidos por un espacio en blanco**.

```xml
<!-- Correct -->
<controls:ImageControl />
<ColumnDefinition Width="Auto" />

<!-- Incorrect -->
<controls:ImageControl/>
<ColumnDefinition Width="Auto"/>
```

### 6. Nomenclatura de elementos (`x:Name`)

Los nombres asignados mediante `x:Name` deben usar un **prefijo** según el tipo de elemento. La siguiente tabla define los prefijos conocidos:

| Elemento                          | Prefijo        |
|-----------------------------------|----------------|
| `Label`                           | `Lbl`          |
| `Span`                            | `Span`         |
| `Entry`, `Editor`                 | `Txt`          |
| `Picker`, `DatePicker`, etc.      | `Pck`          |
| `Button`                          | `Btn`          |
| `Image`                           | `Img`          |
| `CheckBox`                        | `Chk`          |
| `Expander`                        | `Exp`          |
| `StackLayout`                     | `Stack`        |
| `Grid`                            | `Grid`         |
| `ContentView`                     | `Content`      |
| `ScrollView`                      | `Scroll`       |
| `TapGestureRecognizer`            | `Tap`          |

Si el elemento **no aparece en la tabla**, se usará como prefijo el propio nombre del elemento, **eliminando el sufijo `Control`** en caso de que lo tenga.

```xml
<!-- Correct -->
<Label x:Name="LblTitle" />
<Entry x:Name="TxtEmail" />
<Button x:Name="BtnSubmit" />
<Image x:Name="ImgAvatar" />
<Grid x:Name="GridContainer" />
<TapGestureRecognizer x:Name="TapItem" />

<!-- Custom control: "CardControl" -> prefix "Card" (remove "Control" suffix) -->
<controls:CardControl x:Name="CardProfile" />

<!-- Incorrect -->
<Label x:Name="TitleLabel" />
<Button x:Name="SubmitButton" />
<controls:CardControl x:Name="CardControlProfile" />
```
