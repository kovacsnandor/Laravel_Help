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