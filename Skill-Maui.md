# Skill - .NET MAUI

Mejores prácticas y convenciones para proyectos .NET, con especialización en .NET MAUI.

---

## Archivos XAML

### 1. Eliminar la declaración XML

La línea de declaración XML **siempre debe ser eliminada** de los archivos `.xaml`:

```xml
<!-- ❌ ELIMINAR esta línea -->
<?xml version="1.0" encoding="utf-8" ?>
```

### 2. Orden de propiedades en el elemento raíz

Las propiedades del elemento raíz deben organizarse en **dos bloques ordenados alfabéticamente**, en este orden:

1. **Bloque `xmlns:`** — Espacios de nombres. Ordenados alfabéticamente entre sí.
2. **Bloque `x:`** — Propiedades del namespace `x:`. Ordenadas alfabéticamente entre sí.

Cada bloque se trata como independiente para la ordenación.

```xml
<!-- ✅ Correcto -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:local="clr-namespace:MyApp"
    xmlns:viewmodels="clr-namespace:MyApp.ViewModels"
    x:Class="MyApp.MainPage"
    x:Name="Root">

<!-- ❌ Incorrecto (desordenado, x: mezclado con xmlns:) -->
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
<!-- ❌ Si "toolkit" no se usa en ningún sitio del XAML, eliminar su declaración -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"
    x:Class="MyApp.MainPage">

<!-- ✅ Solo declarar lo que se usa -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    x:Class="MyApp.MainPage">
```

### 4. Separación entre elementos con líneas en blanco

Cada elemento hijo debe tener **una línea en blanco antes y después** para mejorar la legibilidad. La excepción son las **propiedades adjuntas** del elemento padre (como `Grid.ColumnDefinitions`, `Grid.RowDefinitions`, etc.), que van pegadas directamente a su elemento sin línea en blanco de por medio.

```xml
<!-- ✅ Correcto -->
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <controls:ImageControl />

    <controls:LabelControl Grid.Column="1" />

</Grid>

<!-- ❌ Incorrecto (sin separación entre elementos) -->
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>
    <controls:ImageControl />
    <controls:LabelControl Grid.Column="1" />
</Grid>

<!-- ❌ Incorrecto (línea en blanco entre el elemento y sus propiedades adjuntas) -->
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
<!-- ✅ Correcto -->
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
<!-- ✅ Correcto -->
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
<!-- ✅ Correcto -->
<controls:ImageControl />
<ColumnDefinition Width="Auto" />

<!-- ❌ Incorrecto -->
<controls:ImageControl/>
<ColumnDefinition Width="Auto"/>
```
