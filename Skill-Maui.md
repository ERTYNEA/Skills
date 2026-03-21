# Skill - .NET MAUI

> **Versión:** V.0.1  
> Conjunto de reglas, mejores prácticas y convenciones para proyectos .NET, con especialización en .NET MAUI.

---

## Reglas globales

### Idioma inglés

Todo el código debe escribirse **siempre en inglés**: nombres de clases, propiedades, métodos, variables, comentarios, etc. Independientemente del tipo de archivo o del idioma del proyecto.

### Convenciones de nomenclatura

| Elemento                                      | Convención       | Ejemplo                    |
|-----------------------------------------------|------------------|----------------------------|
| **Constantes**                                | `UPPER_CASE`     | `MAX_RETRY_COUNT`          |
| **Propiedades y métodos** (clases y XAML)     | `PascalCase`     | `UserName`, `GetUserData`  |
| **Variables** (`var`, parámetros, campos, etc.) | `camelCase`      | `userName`, `retryCount` (sin prefijo `_`) |

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

### 7. Orden de propiedades en los elementos

Las propiedades de un elemento deben organizarse en **grupos**, siguiendo este orden:

1. **`x:Name`** y **estilo** (`Style`, `StyleType`, etc.)
2. **`Grid.Row`** y **`Grid.Column`**
3. **Propiedades genéricas** (comunes a muchos elementos: `IsVisible`, `IsEnabled`, `BackgroundColor`, `Opacity`, etc.)
4. **Propiedades específicas** (propias del elemento: `Text`, `Source`, `Command`, `Placeholder`, `FontSize`, `TextColor`, etc.)
5. **`HeightRequest`** y **`WidthRequest`**
6. **`Margin`** y **`Padding`**
7. **`VerticalOptions`** y **`HorizontalOptions`**

#### Reglas de formato

- **Cada grupo ocupa una línea.** Las propiedades que pertenecen al mismo grupo van juntas en la misma línea.
- Si una línea crece demasiado, se puede dividir el grupo en varias líneas.
- Tanto las propiedades genéricas como las específicas se ordenan pensando **de afuera hacia adentro** (cómo afectan al elemento, de lo más externo a lo más interno).

```xml
<!-- Correct -->
<controls:LabelControl
    x:Name="LblTitle" Style="{StaticResource TitleStyle}"
    Grid.Row="1" Grid.Column="0"
    IsVisible="{Binding IsVisible}"
    Text="{Binding Title}" FontSize="18" TextColor="White"
    HeightRequest="40" WidthRequest="200"
    Margin="8,0"
    VerticalOptions="Center" HorizontalOptions="Start" />

<!-- Correct -->
<Image
    x:Name="ImgIcon"
    Grid.Row="0"
    Source="icon.png"
    WidthRequest="24" />

<!-- Incorrect (disordered properties) -->
<controls:LabelControl
    Margin="8,0"
    Text="{Binding Title}"
    x:Name="LblTitle"
    Grid.Row="1"
    VerticalOptions="Center"
    Style="{StaticResource TitleStyle}"
    IsVisible="{Binding IsVisible}" />
```

### 8. Declarar explícitamente `Grid.Row` y `Grid.Column` desde 0

Cuando un `Grid` tiene `RowDefinitions`, todos sus elementos hijos deben declarar `Grid.Row` **incluyendo el valor `0`**. Lo mismo aplica para `Grid.Column` cuando el `Grid` tiene `ColumnDefinitions`.

```xml
<!-- Correct -->
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
    </Grid.ColumnDefinitions>

    <controls:ImageControl Grid.Row="0" Grid.Column="0" />

    <controls:LabelControl Grid.Row="0" Grid.Column="1" />

    <controls:ButtonControl Grid.Row="1" Grid.Column="0" />

</Grid>

<!-- Incorrect (missing Grid.Row="0" and Grid.Column="0") -->
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="*" />
    </Grid.ColumnDefinitions>

    <controls:ImageControl />

    <controls:LabelControl Grid.Column="1" />

    <controls:ButtonControl Grid.Row="1" />

</Grid>
```

---

## Archivos C# (`.cs`)

### 1. Namespace con file-scoped declaration

La primera línea del archivo será el `namespace`, usando la sintaxis **file-scoped** (sin llaves `{}`). El resto del código no se encapsula dentro del namespace.

```csharp
// Correct
namespace App.UI.Features;

// Incorrect (block-scoped)
namespace App.UI.Features
{
    // ...
}
```

### 2. Usings ordenados y sin duplicados

Después del `namespace`, se escriben las directivas `using`. Deben estar **ordenadas alfabéticamente** y se deben **eliminar las que no se estén utilizando**.

```csharp
// Correct
namespace App.UI.Features;

using App.Core.Models;
using App.UI.Controls;
using CommunityToolkit.Mvvm.ComponentModel;
using Microsoft.Maui.Controls;

// Incorrect (unordered, unused usings)
namespace App.UI.Features;

using Microsoft.Maui.Controls;
using App.UI.Controls;
using System.Diagnostics; // Not used anywhere
using App.Core.Models;
```

### 3. Una clase por fichero

Cada archivo `.cs` debe contener **una única clase**. El nombre del archivo debe coincidir con el nombre de la clase.

```csharp
// Correct — File: UserProfilePage.cs
namespace App.UI.Features;

using App.Core.Models;

public partial class UserProfilePage : ContentPage
{
    // ...
}
```

### 4. Orden del contenido de una clase

El contenido de una clase se organiza en el siguiente orden:

1. **Propiedades de interfaces**
2. **Constructor**
3. **Destructor** (si existe)
4. **Propiedades Reactive**
5. **Propiedades ReactiveCommand**
6. **Resto de propiedades**
7. **Métodos**

Todas las secciones de propiedades y métodos siguen la misma regla de ordenación: se agrupan por **nivel de acceso** (de menos restrictivo a más restrictivo: `public` → `internal` → `protected internal` → `protected` → `private`) y dentro de cada grupo se ordenan **alfabéticamente**. Las propiedades y métodos no utilizados **deben eliminarse**.

### 5. Propiedades de interfaces

Son las primeras que se escriben en la clase. Se declaran al inicio, antes del constructor.

```csharp
public partial class UserProfilePage : ContentPage
{
    public IAuthService AuthService { get; set; }
    public INavigationService NavigationService { get; set; }

    private ILoggerService loggerService;

    // ...
}
```

### 6. Constructor

Va inmediatamente después de las propiedades de interfaces. Lo primero que hace el constructor es **inicializar las propiedades de interfaces**, en el **mismo orden en el que fueron declaradas**.

```csharp
public partial class UserProfilePage : ContentPage
{
    public IAuthService AuthService { get; set; }
    public INavigationService NavigationService { get; set; }

    private ILoggerService loggerService;

    public UserProfilePage(
        IAuthService authService,
        INavigationService navigationService,
        ILoggerService loggerService)
    {
        AuthService = authService;
        NavigationService = navigationService;
        this.loggerService = loggerService;

        // ... other initialization
    }

    // ...
}
```

### 7. Destructor

Si la clase tiene destructor, este se escribe **inmediatamente después del constructor**.

```csharp
    public UserProfilePage(ILoggerService loggerService)
    {
        this.loggerService = loggerService;
    }

    ~UserProfilePage()
    {
        // ... cleanup
    }

    // ...
```

### 8. Propiedades Reactive y ReactiveCommand

Después del destructor (o del constructor si no hay destructor) se escriben las propiedades **Reactive**, seguidas de las propiedades **ReactiveCommand**. Ambos grupos se ordenan por nivel de acceso y alfabéticamente. Los atributos como `[Reactive]` se escriben en una **línea separada**, encima de la propiedad a la que afectan.

```csharp
    // Reactive properties
    [Reactive]
    public string Email { get; set; }
    [Reactive]
    public string UserName { get; set; }

    [Reactive]
    private bool isEditing;

    // ReactiveCommand properties
    public ReactiveCommand<Unit, Unit> LoadCommand { get; set; }
    public ReactiveCommand<Unit, Unit> SaveCommand { get; set; }

    private ReactiveCommand<Unit, Unit> validateCommand;
```

### 9. Resto de propiedades

Por último se escriben el resto de propiedades que no encajan en las categorías anteriores, siguiendo la misma ordenación por nivel de acceso y alfabéticamente.

```csharp
    public string Title { get; set; }

    protected int PageIndex { get; set; }

    private bool isLoading;
```

### 10. Métodos

Después de todas las propiedades se escriben los **métodos**. Se ordenan por nivel de acceso (de menos restrictivo a más restrictivo) y **alfabéticamente** dentro de cada grupo.

```csharp
    public void LoadData()
    {
        // ...
    }

    public void SaveData()
    {
        // ...
    }

    protected void OnPropertyChanged()
    {
        // ...
    }

    private void InitializeComponents()
    {
        // ...
    }

    private void ValidateInput()
    {
        // ...
    }
```

### 11. Sufijo `Async` en métodos asíncronos

Los métodos `async` deben tener siempre el sufijo **`Async`** al final de su nombre.

```csharp
// Correct
public async Task LoadDataAsync()
{
    // ...
}

private async Task<bool> ValidateInputAsync()
{
    // ...
}

// Incorrect (missing Async suffix)
public async Task LoadData()
{
    // ...
}

private async Task<bool> ValidateInput()
{
    // ...
}
```

### Ejemplo completo

```csharp
// File: UserProfilePage.cs
namespace App.UI.Features;

using App.Core.Interfaces;
using App.Core.Models;
using ReactiveUI;

public partial class UserProfilePage : ContentPage
{
    // Interface properties
    public IAuthService AuthService { get; set; }
    public INavigationService NavigationService { get; set; }

    private ILoggerService loggerService;

    // Constructor
    public UserProfilePage(
        IAuthService authService,
        INavigationService navigationService,
        ILoggerService loggerService)
    {
        AuthService = authService;
        NavigationService = navigationService;
        this.loggerService = loggerService;
    }

    // Destructor
    ~UserProfilePage()
    {
        // ... cleanup
    }

    // Reactive properties
    [Reactive]
    public string Email { get; set; }
    [Reactive]
    public string UserName { get; set; }

    [Reactive]
    private bool isEditing;

    // ReactiveCommand properties
    public ReactiveCommand<Unit, Unit> LoadCommand { get; set; }
    public ReactiveCommand<Unit, Unit> SaveCommand { get; set; }

    // Other properties
    public string Title { get; set; }

    protected int PageIndex { get; set; }

    private bool isLoading;

    // Methods
    public void LoadData()
    {
        // ...
    }

    public void SaveData()
    {
        // ...
    }

    protected void OnPropertyChanged()
    {
        // ...
    }

    private void InitializeComponents()
    {
        // ...
    }

    private void ValidateInput()
    {
        // ...
    }
}
```

