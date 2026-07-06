# Skill - .NET MAUI

> **Versión:** V.0.4.0
> Conjunto de reglas, mejores prácticas y convenciones para proyectos .NET, con especialización en .NET MAUI.

---

## Reglas globales

### Idioma inglés

Todo el código debe escribirse **siempre en inglés**: nombres de clases, propiedades, métodos, variables, comentarios, etc. Independientemente del tipo de archivo o del idioma del proyecto.

### Comentarios actualizados y concisos

Todos los comentarios deben revisarse para garantizar que siguen describiendo el estado actual del código. Si una refactorización, cambio de flujo o ajuste funcional deja un comentario desactualizado, el comentario debe actualizarse. Los comentarios deben estar escritos en inglés, ser breves y aportar contexto útil sin repetir de forma evidente lo que ya expresa el código.

### Indentación

La indentación se hará con **tabuladores** (`tab`) y ancho visual de 4 espacios en todos los tipos de archivo del proyecto.

### Organización recomendada por features

Cuando un proyecto esté organizado por pantallas, flujos funcionales o módulos, se recomienda agrupar cada feature en su propia carpeta con sus elementos relacionados. Si el proyecto ya usa otra arquitectura clara, se debe respetar la estructura existente.

---

## Archivos XAML

### 1. Eliminar la declaración XML

La línea de declaración XML **siempre debe ser eliminada** de los archivos `.xaml`:

```xml
<!-- REMOVE this line -->
<?xml version="1.0" encoding="utf-8" ?>
```

### 2. Primera propiedad en la misma línea del elemento

La primera propiedad de un elemento XAML debe escribirse **en la misma línea** que la etiqueta de apertura del elemento. Las propiedades adicionales pueden continuar en líneas siguientes con la indentación correspondiente.

```xml
<!-- Correct -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    x:Class="MyApp.MainPage">

<!-- Correct -->
<controls:LabelControl x:Name="LblTitle"
    Text="{Binding Title}" />

<!-- Incorrect (first property on a new line) -->
<ContentPage
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    x:Class="MyApp.MainPage">

<!-- Incorrect (first property on a new line) -->
<controls:LabelControl
    x:Name="LblTitle"
    Text="{Binding Title}" />
```

### 3. Orden de propiedades en el elemento raíz

Las propiedades del elemento raíz deben organizarse en **dos bloques ordenados alfabéticamente**, en este orden:

1. **Bloque `xmlns:`** — Espacios de nombres. Ordenados alfabéticamente entre sí.
2. **Bloque `x:`** — Propiedades del namespace `x:`. Ordenadas alfabéticamente entre sí.

Cada bloque se trata como independiente para la ordenación.

```xml
<!-- Correct -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:local="clr-namespace:MyApp"
    xmlns:viewmodels="clr-namespace:MyApp.ViewModels"
    x:Class="MyApp.MainPage"
    x:Name="Root">

<!-- Incorrect (unordered, x: mixed with xmlns:) -->
<ContentPage xmlns:viewmodels="clr-namespace:MyApp.ViewModels"
    x:Class="MyApp.MainPage"
    xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:local="clr-namespace:MyApp"
    x:Name="Root">
```

### 4. Eliminar espacios de nombres no utilizados

Toda declaración `xmlns:` que no se esté utilizando en el cuerpo del archivo XAML **debe ser eliminada**.

```xml
<!-- If "toolkit" is not used anywhere in the XAML, remove its declaration -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    xmlns:toolkit="http://schemas.microsoft.com/dotnet/2022/maui/toolkit"
    x:Class="MyApp.MainPage">

<!-- Only declare what is used -->
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
    x:Class="MyApp.MainPage">
```

### 5. Separación entre elementos con líneas en blanco

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

### 6. Espacio antes del cierre de elementos autocerrados

Los cierres de elementos autocerrados (`/>`) deben ir **siempre precedidos por un espacio en blanco**.

```xml
<!-- Correct -->
<controls:ImageControl />
<ColumnDefinition Width="Auto" />

<!-- Incorrect -->
<controls:ImageControl/>
<ColumnDefinition Width="Auto"/>
```

### 7. Nomenclatura de elementos (`x:Name`)

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

### 8. Orden de propiedades en los elementos

Las propiedades de un elemento deben organizarse en **grupos**, siguiendo este orden:

1. **`x:Name`** y **estilo** (`Style`, `StyleType`, etc.)
2. **`Grid.Row`** y **`Grid.Column`**
3. **Propiedades genéricas** (comunes a muchos elementos: `IsVisible`, `IsEnabled`, `BackgroundColor`, `Opacity`, etc.)
4. **Propiedades específicas** (propias del elemento: `Text`, `Source`, `Command`, `Placeholder`, `FontSize`, `TextColor`, etc.)
5. **`HeightRequest`** y **`WidthRequest`**
6. **`Margin`** y **`Padding`**
7. **`HorizontalOptions`** y **`VerticalOptions`**

#### Reglas de formato

- **Cada grupo ocupa una línea.** Las propiedades que pertenecen al mismo grupo van juntas en la misma línea.
- Si una línea crece demasiado, se puede dividir el grupo en varias líneas.
- Tanto las propiedades genéricas como las específicas se ordenan pensando **de afuera hacia adentro** (cómo afectan al elemento, de lo más externo a lo más interno).

```xml
<!-- Correct -->
<controls:LabelControl x:Name="LblTitle" Style="{StaticResource TitleStyle}"
    Grid.Row="1" Grid.Column="0"
    IsVisible="{Binding IsVisible}"
    Text="{Binding Title}" FontSize="18" TextColor="White"
    HeightRequest="40" WidthRequest="200"
    Margin="8,0"
    HorizontalOptions="Start" VerticalOptions="Center" />

<!-- Correct -->
<Image x:Name="ImgIcon"
    Grid.Row="0"
    Source="icon.png"
    WidthRequest="24" />

<!-- Incorrect (disordered properties) -->
<controls:LabelControl Margin="8,0"
    Text="{Binding Title}"
    x:Name="LblTitle"
    Grid.Row="1"
    VerticalOptions="Center"
    Style="{StaticResource TitleStyle}"
    IsVisible="{Binding IsVisible}" />
```

### 9. Declarar explícitamente `Grid.Row` y `Grid.Column` desde 0

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

## Archivos Code-Behind (`.xaml.cs`)

### Convenciones de nomenclatura C#

| Elemento                                      | Convención       | Ejemplo                    |
|-----------------------------------------------|------------------|----------------------------|
| **Campos privados y constantes privadas**     | `camelCase`      | `maxRetryCount`            |
| **Propiedades y métodos**                     | `PascalCase`     | `UserName`, `GetUserData`  |
| **Variables** (`var`, parámetros, campos, etc.) | `camelCase`      | `userName`, `retryCount` (sin prefijo `_`) |

### Formato C# base

- Usar `var` solo cuando el tipo sea evidente por la asignación. Si el tipo no es claro, usar el tipo explícito.
- No usar `this.` salvo que sea necesario para desambiguar o para invocar extension methods que lo requieran.
- No dejar bloques `catch` vacíos. Como mínimo, registrar la excepción, propagarla o justificar explícitamente por qué se ignora.

### Documentación XML de métodos

Los métodos medianos, grandes o con complejidad considerable deben incluir documentación XML con `/// <summary>`. Los métodos pequeños y evidentes no deben recibir documentación XML innecesaria solo por aplicar esta regla.

Si un método se mueve o reordena y ya tiene documentación XML, esa documentación forma parte del método y debe moverse junto con él. No se debe separar, borrar ni dejar atrás el `summary` al reorganizar métodos.

Toda documentación XML existente debe revisarse para garantizar que sigue reflejando el comportamiento actual del método. El `summary` debe explicar de forma clara y resumida qué hace el método. Cuando el método tenga parámetros, cada parámetro debe documentarse con `/// <param name="...">`. Cuando el método devuelva un valor significativo, debe documentarse con `/// <returns>`. El orden recomendado es: `summary`, después `param` en el mismo orden de la firma del método, y finalmente `returns`.

```csharp
/// <summary>
/// Loads the user profile data.
/// </summary>
/// <param name="userId">Identifier of the user to load.</param>
/// <returns>The loaded user profile.</returns>
public Task<UserProfile> LoadUserProfileAsync(string userId)
{
    // ...
}
```

### Clases base del proyecto

Si el proyecto define clases base comunes para `Page`, `ViewModel`, `Popup`, `ContentView`, servicios u otros componentes, deben usarse de forma consistente. No introducir una clase base distinta si contradice el patrón existente del proyecto.

### Sufijo `Async` en métodos que devuelven tareas

Los métodos que devuelvan `Task`, `Task<T>`, `ValueTask` o `ValueTask<T>` deben tener siempre el sufijo **`Async`** al final de su nombre, aunque no estén marcados con el modificador `async`.

### 1. Namespace con file-scoped declaration

La primera línea del archivo será el `namespace`, usando la sintaxis **file-scoped** (sin llaves `{}`).

```csharp
// Correct
namespace App.UI.Controls;

// Incorrect (block-scoped)
namespace App.UI.Controls
{
    // ...
}
```

### 2. Usings ordenados y sin duplicados

Después del `namespace`, se escriben las directivas `using`. Deben estar **ordenadas alfabéticamente** y se deben **eliminar las que no se estén utilizando**.

```csharp
// Correct
namespace App.UI.Controls;

using App.Core.Interfaces;
using ReactiveUI;
using System.Reactive.Disposables;
using System.Windows.Input;

// Incorrect (unordered, unused usings)
namespace App.UI.Controls;

using ReactiveUI;
using System.Windows.Input;
using System.Diagnostics; // Not used anywhere
using App.Core.Interfaces;
```

### 3. Declaración de la clase

Cada archivo `.xaml.cs` debe contener **una única clase**. El nombre del archivo debe coincidir con el nombre de la clase.

```csharp
// Correct — File: UserCardControl.xaml.cs
namespace App.UI.Controls;

using App.Core.Interfaces;

public partial class UserCardControl : ReactiveContentView<UserCardViewModel>
{
    // ...
}
```

### 4. Llaves en estructuras de control

Esta regla aplica al código C# de archivos `.xaml.cs`.

Cuando una estructura de control contenga una única instrucción de una sola línea, **no deben utilizarse llaves**. La estructura de control se escribe en una línea y la instrucción interna en la línea siguiente, con la indentación correspondiente.

Las estructuras de control afectadas por esta regla son: `if`, `else if`, `else`, `for`, `foreach`, `while` y `do while`.

Los bloques `try`, `catch` y `finally` **siempre deben mantener llaves**, aunque contengan una única instrucción. La estructura `switch` queda fuera de esta regla porque su sintaxis requiere un bloque con llaves.

```csharp
// Correct
if (isValid)
    SaveData();
else if (isPending)
    QueueData();
else
    ClearData();

for (var index = 0; index < items.Count; index++)
    ProcessItem(items[index]);

foreach (var item in items)
    ProcessItem(item);

while (reader.Read())
    ProcessRow(reader);

do
    retryCount++;
while (ShouldRetry(retryCount));

// Incorrect (single instruction with braces)
if (isValid)
{
    SaveData();
}
else if (isPending)
{
    QueueData();
}
else
{
    ClearData();
}

foreach (var item in items)
{
    ProcessItem(item);
}

// Correct (try/catch/finally always keep braces)
try
{
    SaveData();
}
catch (Exception exception)
{
    LogError(exception);
}
finally
{
    StopLoading();
}

// Incorrect (try/catch/finally without braces)
try
    SaveData();
catch (Exception exception)
    LogError(exception);
finally
    StopLoading();
```

### 5. Orden del contenido de una clase

El contenido de una clase code-behind se organiza en el siguiente orden:

1. **Propiedades BindableProperty**
2. **Propiedades de interfaces**
3. **Resto de propiedades**
4. **Propiedades ResourceDictionary**
5. **Constructor**
6. **Destructor** (si existe)
7. **Get/Set de BindableProperty** y **OnPropertyChanged**
8. **OnActivated** (si existen bindings al ViewModel)
9. **Métodos**

Las propiedades `BindableProperty` deben nombrarse siempre con el sufijo **`Property`** (e.g. `TextProperty`, `CommandProperty`).

Todas las secciones de propiedades siguen la misma regla de ordenación: se agrupan por **nivel de acceso** (de menos restrictivo a más restrictivo: `public` → `internal` → `protected internal` → `protected` → `private`) y dentro de cada grupo se ordenan **alfabéticamente**. Las propiedades y métodos no utilizados **deben eliminarse**.

### 6. Constructor

Va inmediatamente después de todas las propiedades. Lo primero que hace el constructor es **inicializar las propiedades de interfaces**, en el **mismo orden en el que fueron declaradas**, y **antes de `InitializeComponent()`**. Las propiedades de interfaces no utilizadas deben eliminarse.

El nombre de cada propiedad de interfaz se forma a partir del nombre de la interfaz **eliminando el prefijo `I`** y usando **`camelCase`** (convención de variables).

```csharp
public partial class UserCardControl : ReactiveContentView<UserCardViewModel>
{
    // ...properties...

    private IAuthService authService;
    private INavigationService navigationService;

    public UserCardControl(
        IAuthService authService,
        INavigationService navigationService)
    {
        this.authService = authService;
        this.navigationService = navigationService;

        InitializeComponent();
    }

    // ...
}
```

### 7. Destructor

Si la clase tiene destructor, este se escribe **inmediatamente después del constructor**.

```csharp
    public UserCardControl(INavigationService navigationService)
    {
        this.navigationService = navigationService;

        InitializeComponent();
    }

    ~UserCardControl()
    {
        // ... cleanup
    }

    // ...
```

### 8. Get/Set de BindableProperty y OnPropertyChanged

Después del destructor (o del constructor si no hay destructor) se escriben los **get/set** de cada `BindableProperty`, en el **mismo orden en que fueron declaradas**. Se debe revisar la **nulabilidad** del tipo en el getter y setter.

A continuación, si existen `BindableProperty`, se instancia o sobreescribe el método **`OnPropertyChanged`** para gestionar los cambios de propiedades:

- El orden de los bloques `if`/`else if` sigue el **mismo orden de declaración** de las `BindableProperty`.
- Si la acción cabe en **una sola línea**, se escribe en la línea siguiente, sin llaves.
- Si la acción necesita **más de una línea**, se delega a un **método privado** cuyo nombre será el de la `BindableProperty` seguido del sufijo `Changed` (e.g. `CommandProperty` → `CommandPropertyChanged`).
- Se deben **eliminar** las entradas de propiedades que no estén en uso.

```csharp
    // BindableProperty get/set (same order as declared)
    public ICommand? Command
    {
        get => (ICommand?)GetValue(CommandProperty);
        set => SetValue(CommandProperty, value);
    }

    public bool IsActive
    {
        get => (bool)GetValue(IsActiveProperty);
        set => SetValue(IsActiveProperty, value);
    }

    public string? Text
    {
        get => (string?)GetValue(TextProperty);
        set => SetValue(TextProperty, value);
    }

    // OnPropertyChanged (same order as BindableProperty declarations)
    protected override void OnPropertyChanged(string propertyName = null)
    {
        base.OnPropertyChanged(propertyName);

        if (propertyName == CommandProperty.PropertyName)
            CommandPropertyChanged();
        else if (propertyName == IsActiveProperty.PropertyName)
            BtnMain.IsEnabled = IsActive;
        else if (propertyName == TextProperty.PropertyName)
            LblMain.Text = Text;
    }
```

### 9. OnActivated

Si se necesitan bindear elementos del XAML al `ViewModel`, se hará en el método **`OnActivated`** (instanciándolo o sobreescribiéndolo). Dentro se usará `disposables.Add(...)` para cada binding.

Los bindings se organizan en **familias independientes**, en este orden:

1. **`OneWayBind`** — Bindings de solo lectura del ViewModel hacia la vista.
2. **`Bind`** — Bindings bidireccionales.
3. **`Subscribe`** — Suscripciones a cambios con `WhenAnyValue`. Siempre se ejecutan en el hilo principal usando `.ObserveOn(RxApp.MainThreadScheduler)`. Si la suscripción cabe en una línea se escribe inline; si necesita más de una línea se delega a un método cuyo nombre será la propiedad del ViewModel seguida del sufijo `Subscribe` (e.g. propiedad `Items` → `ItemsSubscribe`).
4. **`BindCommand`** — Bindings de comandos.

Dentro de cada familia, los bindings se ordenan **alfabéticamente por la propiedad del ViewModel** a la que están vinculados.

```csharp
    protected override void OnActivated(CompositeDisposable disposables)
    {
        base.OnActivated(disposables);

        // OneWayBind
        disposables.Add(this.OneWayBind(ViewModel, vm => vm.Icon, v => v.ImgIcon.Source));
        disposables.Add(this.OneWayBind(ViewModel, vm => vm.Title, v => v.LblTitle.Text));

        // Bind
        disposables.Add(this.Bind(ViewModel, vm => vm.IsEditing, v => v.TxtName.IsEnabled));
        disposables.Add(this.Bind(ViewModel, vm => vm.UserName, v => v.TxtName.Text));

        // Subscribe
        disposables.Add(ViewModel.WhenAnyValue(vm => vm.Items)
            .ObserveOn(RxApp.MainThreadScheduler)
            .Subscribe(ItemsSubscribe));
        disposables.Add(ViewModel.WhenAnyValue(vm => vm.Status)
            .ObserveOn(RxApp.MainThreadScheduler)
            .Subscribe(status => LblStatus.Text = status));

        // BindCommand
        disposables.Add(this.BindCommand(ViewModel, vm => vm.DeleteCommand, v => v.BtnDelete));
        disposables.Add(this.BindCommand(ViewModel, vm => vm.SaveCommand, v => v.BtnSave));
    }
```

### 10. Métodos

Después de `OnActivated` se escriben los **métodos**. Se ordenan por nivel de acceso (de menos restrictivo a más restrictivo) y **alfabéticamente** dentro de cada grupo.

```csharp
    private void CommandPropertyChanged()
    {
        BtnMain.Command = Command;
        BtnMain.IsEnabled = Command != null;
    }

    private void ItemsSubscribe(IEnumerable<string> items)
    {
        StackItems.Children.Clear();
        foreach (var item in items)
            StackItems.Children.Add(new Label { Text = item });
    }
```

### Ejemplo completo

```csharp
// File: UserCardControl.xaml.cs
namespace App.UI.Controls;

using App.Core.Interfaces;
using ReactiveUI;
using ReactiveUI.Maui;
using System.Reactive.Disposables;
using System.Windows.Input;

public partial class UserCardControl : ReactiveContentView<UserCardViewModel>
{
    // BindableProperty
    public static readonly BindableProperty CommandProperty =
        BindableProperty.Create(nameof(Command), typeof(ICommand), typeof(UserCardControl));
    public static readonly BindableProperty IsActiveProperty =
        BindableProperty.Create(nameof(IsActive), typeof(bool), typeof(UserCardControl));
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create(nameof(Text), typeof(string), typeof(UserCardControl));

    // Interface properties
    private IAuthService authService;
    private INavigationService navigationService;

    // Other properties
    private bool isInitialized;

    // ResourceDictionary
    private ResourceDictionary mainResources;

    // Constructor
    public UserCardControl(
        IAuthService authService,
        INavigationService navigationService)
    {
        this.authService = authService;
        this.navigationService = navigationService;

        InitializeComponent();
    }

    // Destructor
    ~UserCardControl()
    {
        // ... cleanup
    }

    // BindableProperty get/set
    public ICommand? Command
    {
        get => (ICommand?)GetValue(CommandProperty);
        set => SetValue(CommandProperty, value);
    }

    public bool IsActive
    {
        get => (bool)GetValue(IsActiveProperty);
        set => SetValue(IsActiveProperty, value);
    }

    public string? Text
    {
        get => (string?)GetValue(TextProperty);
        set => SetValue(TextProperty, value);
    }

    // OnPropertyChanged
    protected override void OnPropertyChanged(string propertyName = null)
    {
        base.OnPropertyChanged(propertyName);

        if (propertyName == CommandProperty.PropertyName)
            CommandPropertyChanged();
        else if (propertyName == IsActiveProperty.PropertyName)
            BtnMain.IsEnabled = IsActive;
        else if (propertyName == TextProperty.PropertyName)
            LblMain.Text = Text;
    }

    // OnActivated
    protected override void OnActivated(CompositeDisposable disposables)
    {
        base.OnActivated(disposables);

        // OneWayBind
        disposables.Add(this.OneWayBind(ViewModel, vm => vm.Icon, v => v.ImgIcon.Source));
        disposables.Add(this.OneWayBind(ViewModel, vm => vm.Title, v => v.LblTitle.Text));

        // Bind
        disposables.Add(this.Bind(ViewModel, vm => vm.IsEditing, v => v.TxtName.IsEnabled));
        disposables.Add(this.Bind(ViewModel, vm => vm.UserName, v => v.TxtName.Text));

        // Subscribe
        disposables.Add(ViewModel.WhenAnyValue(vm => vm.Items)
            .ObserveOn(RxApp.MainThreadScheduler)
            .Subscribe(ItemsSubscribe));
        disposables.Add(ViewModel.WhenAnyValue(vm => vm.Status)
            .ObserveOn(RxApp.MainThreadScheduler)
            .Subscribe(status => LblStatus.Text = status));

        // BindCommand
        disposables.Add(this.BindCommand(ViewModel, vm => vm.DeleteCommand, v => v.BtnDelete));
        disposables.Add(this.BindCommand(ViewModel, vm => vm.SaveCommand, v => v.BtnSave));
    }

    // Methods
    private void CommandPropertyChanged()
    {
        BtnMain.Command = Command;
        BtnMain.IsEnabled = Command != null;
    }

    private void ItemsSubscribe(IEnumerable<string> items)
    {
        StackItems.Children.Clear();
        foreach (var item in items)
            StackItems.Children.Add(new Label { Text = item });
    }
}
```

---

## Archivos C# (`.cs`)

### Convenciones de nomenclatura C#

| Elemento                                      | Convención       | Ejemplo                    |
|-----------------------------------------------|------------------|----------------------------|
| **Campos privados y constantes privadas**     | `camelCase`      | `maxRetryCount`            |
| **Propiedades y métodos**                     | `PascalCase`     | `UserName`, `GetUserData`  |
| **Variables** (`var`, parámetros, campos, etc.) | `camelCase`      | `userName`, `retryCount` (sin prefijo `_`) |

### Formato C# base

- Usar `var` solo cuando el tipo sea evidente por la asignación. Si el tipo no es claro, usar el tipo explícito.
- No usar `this.` salvo que sea necesario para desambiguar o para invocar extension methods que lo requieran.
- No dejar bloques `catch` vacíos. Como mínimo, registrar la excepción, propagarla o justificar explícitamente por qué se ignora.

### Documentación XML de métodos

Los métodos medianos, grandes o con complejidad considerable deben incluir documentación XML con `/// <summary>`. Los métodos pequeños y evidentes no deben recibir documentación XML innecesaria solo por aplicar esta regla.

Si un método se mueve o reordena y ya tiene documentación XML, esa documentación forma parte del método y debe moverse junto con él. No se debe separar, borrar ni dejar atrás el `summary` al reorganizar métodos.

Toda documentación XML existente debe revisarse para garantizar que sigue reflejando el comportamiento actual del método. El `summary` debe explicar de forma clara y resumida qué hace el método. Cuando el método tenga parámetros, cada parámetro debe documentarse con `/// <param name="...">`. Cuando el método devuelva un valor significativo, debe documentarse con `/// <returns>`. El orden recomendado es: `summary`, después `param` en el mismo orden de la firma del método, y finalmente `returns`.

```csharp
/// <summary>
/// Loads the user profile data.
/// </summary>
/// <param name="userId">Identifier of the user to load.</param>
/// <returns>The loaded user profile.</returns>
public Task<UserProfile> LoadUserProfileAsync(string userId)
{
    // ...
}
```

### Clases base del proyecto

Si el proyecto define clases base comunes para `Page`, `ViewModel`, `Popup`, `ContentView`, servicios u otros componentes, deben usarse de forma consistente. No introducir una clase base distinta si contradice el patrón existente del proyecto.

### Sufijo `Async` en métodos que devuelven tareas

Los métodos que devuelvan `Task`, `Task<T>`, `ValueTask` o `ValueTask<T>` deben tener siempre el sufijo **`Async`** al final de su nombre, aunque no estén marcados con el modificador `async`.

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

### 4. Llaves en estructuras de control

Esta regla aplica al código C# de archivos `.cs`.

Cuando una estructura de control contenga una única instrucción de una sola línea, **no deben utilizarse llaves**. La estructura de control se escribe en una línea y la instrucción interna en la línea siguiente, con la indentación correspondiente.

Las estructuras de control afectadas por esta regla son: `if`, `else if`, `else`, `for`, `foreach`, `while` y `do while`.

Los bloques `try`, `catch` y `finally` **siempre deben mantener llaves**, aunque contengan una única instrucción. La estructura `switch` queda fuera de esta regla porque su sintaxis requiere un bloque con llaves.

```csharp
// Correct
if (isValid)
    SaveData();
else if (isPending)
    QueueData();
else
    ClearData();

for (var index = 0; index < items.Count; index++)
    ProcessItem(items[index]);

foreach (var item in items)
    ProcessItem(item);

while (reader.Read())
    ProcessRow(reader);

do
    retryCount++;
while (ShouldRetry(retryCount));

// Incorrect (single instruction with braces)
if (isValid)
{
    SaveData();
}
else if (isPending)
{
    QueueData();
}
else
{
    ClearData();
}

foreach (var item in items)
{
    ProcessItem(item);
}

// Correct (try/catch/finally always keep braces)
try
{
    SaveData();
}
catch (Exception exception)
{
    LogError(exception);
}
finally
{
    StopLoading();
}

// Incorrect (try/catch/finally without braces)
try
    SaveData();
catch (Exception exception)
    LogError(exception);
finally
    StopLoading();
```

### 5. Orden del contenido de una clase

El contenido de una clase se organiza en el siguiente orden:

1. **Propiedades de interfaces**
2. **Constructor**
3. **Destructor** (si existe)
4. **Propiedades Reactive**
5. **Propiedades ReactiveCommand**
6. **Resto de propiedades**
7. **Métodos**

Todas las secciones de propiedades y métodos siguen la misma regla de ordenación: se agrupan por **nivel de acceso** (de menos restrictivo a más restrictivo: `public` → `internal` → `protected internal` → `protected` → `private`) y dentro de cada grupo se ordenan **alfabéticamente**. Las propiedades y métodos no utilizados **deben eliminarse**.

### 6. Propiedades de interfaces

Son las primeras que se escriben en la clase. Se declaran al inicio, antes del constructor. El nombre de cada propiedad se forma a partir del nombre de la interfaz **eliminando el prefijo `I`** y usando **`camelCase`**.

```csharp
public partial class UserProfilePage : ContentPage
{
    private IAuthService authService;
    private ILoggerService loggerService;
    private INavigationService navigationService;

    // ...
}
```

### 7. Constructor

Va inmediatamente después de las propiedades de interfaces. Lo primero que hace el constructor es **inicializar las propiedades de interfaces**, en el **mismo orden en el que fueron declaradas**.

```csharp
public partial class UserProfilePage : ContentPage
{
    private IAuthService authService;
    private ILoggerService loggerService;
    private INavigationService navigationService;

    public UserProfilePage(
        IAuthService authService,
        ILoggerService loggerService,
        INavigationService navigationService)
    {
        this.authService = authService;
        this.loggerService = loggerService;
        this.navigationService = navigationService;

        // ... other initialization
    }

    // ...
}
```

### 8. Destructor

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

### 9. Propiedades Reactive y ReactiveCommand

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

### 10. Resto de propiedades

Por último se escriben el resto de propiedades que no encajan en las categorías anteriores, siguiendo la misma ordenación por nivel de acceso y alfabéticamente.

```csharp
    public string Title { get; set; }

    protected int PageIndex { get; set; }

    private bool isLoading;
```

### 11. Métodos

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
    private IAuthService authService;
    private ILoggerService loggerService;
    private INavigationService navigationService;

    // Constructor
    public UserProfilePage(
        IAuthService authService,
        ILoggerService loggerService,
        INavigationService navigationService)
    {
        this.authService = authService;
        this.loggerService = loggerService;
        this.navigationService = navigationService;
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
