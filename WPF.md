# VisulaStudio
## Copilot kikapcsolása
- Tools/Options
    - Code Completions Providers (pipák kivesz)
        - [ ] Copilot ... 
        - [ ] IntelliCode ... 

# WPF objektum adatkötés

## WPF application
1. Ezt érdemes indítani új projektnél, sokkal modernebb.

## CommunityToolkit.Mvvm
2. Adatkötéshez talpíteni kell ezt a nuget csomagot:
    - Menüben: `Tools/Nuget Package Manager/Manage nUget Packages for solution`
    - Rákeresni: `CommunityToolkit.Mvvm`

## Az objektum
Példa: Person osztály
Person.class
```cs
//Fontos: ObservableObject-ből kell örökölni
public partial class Person : ObservableObject
{
    //Mezők: mindegyik private
    //A háttében generálódik mindegyikhez egy nagybetűs public Tulajdonság (property)
    //Ez a Person.g.cs osztályban található (valhol, mindegy hol)
    // [ObservableProperty] Attribútum oldja meg az oda-vissza UI frissítést
    [ObservableProperty]
    private string name;

    //[NotifyPropertyChangedFor(nameof(MitIszik))] gondoskodik MitIszik UI frissítésről
    [ObservableProperty]
    [NotifyPropertyChangedFor(nameof(MitIszik))]
    [NotifyPropertyChangedFor(nameof(MitIszikSzin))]
    private int age;

    [ObservableProperty]
    private string city;

    //Tulajdonságok: Számított Tulajdonságok
    //Ezek valmelyik mező tulajdonságától függnek
    public string MitIszik
    {
        get { return this.Age < 18 ? "Kóla" : "Sör"; }
    }

    public Brush MitIszikSzin
    {
        get { return this.Age < 18 ? Brushes.Red : Brushes.Green; }
    }

    //Konstruktor
    public Person(string name, int age, string city)
    {
        this.Name = name;
        this.Age = age;
        this.City = city;
    }
}
```

## Az adatkötés a XAML-ben
Az adatkötés megoldása:
```xml
    <Grid x:Name="myGrid">
        <TextBlock
            d:Text="Name"
            HorizontalAlignment="Left" Margin="41,28,0,0" TextWrapping="Wrap" 
            Text="{Binding Name}" VerticalAlignment="Top"/>
        <TextBlock
            d:Text="Age"
            HorizontalAlignment="Left" Margin="41,62,0,0" TextWrapping="Wrap" 
            Text="{Binding Age}" VerticalAlignment="Top"/>
        <TextBlock
            d:Text="City"
            HorizontalAlignment="Left" Margin="41,103,0,0" TextWrapping="Wrap" 
            Text="{Binding City}" VerticalAlignment="Top"/>
        <TextBlock
            d:Text="Mit ihat"
            HorizontalAlignment="Left" Margin="165,157,0,0" TextWrapping="Wrap" Text="{Binding MitIszik}" VerticalAlignment="Top" 
            Foreground="{Binding MitIszikSzin}"
            />
        <Button
            x:Name="buttonChange"
            Content="Módosítás" HorizontalAlignment="Left" Margin="176,53,0,0" VerticalAlignment="Top" Click="buttonChange_Click"/>
        <Slider
            Value="{Binding Age}"
            HorizontalAlignment="Left" Margin="41,216,0,0" VerticalAlignment="Top" Width="301" Maximum="100" Minimum="1"/>

    </Grid>

```

## Data context beállítása
A xaml kód bihájndjában hozzá kell kötni a Grid-hez:

```cs
public partial class MainWindow : Window
{
    //Példányosítunk egy person objektumot
    private Person person = new Person("Béla", 24, "Szolnok");
    public MainWindow()
    {
        InitializeComponent();
        //Az adatkötéshez A grid dataContext-jéhez hozzá kell rendelni az objektumot
        myGrid.DataContext = this.person;
    }

    private void buttonChange_Click(object sender, RoutedEventArgs e)
    {
        this.person.Name = "Feri";
        this.person.Age = 8;
        this.person.City = "Budapest";
    }
}
```

# Fontos objektumok
## Színezés
- Színnevekkel: Brushes statikus osztállyal.
```c#
myButton.Background = Brushes.LightBlue;
```

- Szín (R,G,B) decimális kódokkal
```c#
// Szín létrehozása (R, G, B)
Color egyediSzin = Color.FromRgb(255, 87, 51); 
// Brush létrehozása a színből
myButton.Background = new SolidColorBrush(egyediSzin);
```

- Szín és átlátszóság (A,R,G,B) decimális kódokkal
```c#
// Szín létrehozása (R, G, B)
Color egyediSzin = Color.FromArgb(100, 255, 87, 51); 
// Brush létrehozása a színből
myButton.Background = new SolidColorBrush(egyediSzin);
```

- (Rh,Gh,Bh) helxa kódokkal
```c#
// Szín létrehozása (R, G, B)
Color egyediSzin = Color.FromArgb(255, 0xFF, 0x57, 0x33); 
// Brush létrehozása a színből
myButton.Background = new SolidColorBrush(egyediSzin);
```


- Hexa kóddal
```c#
var color = (Color)ColorConverter.ConvertFromString("#FF5733");
myButton.Background = new SolidColorBrush(color);
```

## Decimális <-> Hexa konverziók
- Dec -> Hexa
```c#
byte r = 255;
byte g = 87;
byte b = 51;

// "X2" = Hexadecimális, minimum 2 karakteren (vezető nullával)
string hex = $"#{r:X2}{g:X2}{b:X2}";

```

- Hexa -> Dec
```c#
using System.Windows.Media;

string hex = "#FF5733";
Color color = (Color)ColorConverter.ConvertFromString(hex);

byte r = color.R;
byte g = color.G;
byte b = color.B;
```

## sztring és vágólap

- String vágólapra
```c#
using System.Windows;

// ...

string szinKod = "#FF5733";
Clipboard.SetText(szinKod);
```

- String vágólapról
```c#
if (Clipboard.ContainsText())
{
    string vagoLapTartalom = Clipboard.GetText();
    // Itt jöhet a korábban beszélt ColorConverter...
}
```

## Színből színbe
- Egy csatorna színintenzitás
Példa: Fehérből pirosba:
```c#
// intenzitas: 0 (fehér) és 255 (piros) közötti érték
public Color GetRedIntensity(bytle intenzitas)
{
    byte gb = (byte)(255 - intenzitas);   
    return Color.FromRgb(r, gb, gb);
}
```

- Tetszőleg színből színbe: intenzitás: t: 0-1 között
    - t=0 s1
    - t=2 s2

```c#
public Color Lerp(Color s1, Color s2, double t)
{
    byte r = (byte)(s1.R + (s2.R - s1.R) * t);
    byte g = (byte)(s1.G + (s2.G - s1.G) * t);
    byte b = (byte)(s1.B + (s2.B - s1.B) * t);
    return Color.FromRgb(r, g, b);
}

// Használat (Fehértől Pirosig):
Color halvany = Lerp(Colors.White, Colors.Red, 0.5); // 50%-os halványpiros
```