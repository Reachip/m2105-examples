# m2105-snippet

## Création de grilles à l'horizontal et à la verticale

```xml
<Grid.RowDefinitions>
    <RowDefinition Height="19*"/>
</Grid.RowDefinitions>
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="150"/>
    <ColumnDefinition Width="1*"/>
</Grid.ColumnDefinitions>
```


## Binding sur l'ensemble des éléments de la vue

```xml

<Grid DataContext="{Binding SelectedItem, ElementName=bar}">
    <!-- ... -->
    <!-- ... -->
    <!-- ... -->

    <DataGrid Grid.Row="1" Margin="0,10,0,0" ItemsSource="{Binding UnElementDeFoo}" AutoGenerateColumns="False" Grid.ColumnSpan="2" >
        <DataGrid.Columns>
            <DataGridTextColumn Header="UnHeader" Binding="{Binding UnElementDeFoo}" Width="2*"/>
            <DataGridTextColumn Header="UnHeader" Binding="{Binding UnElementDeFoo, StringFormat=C}" Width="Auto"/>
            <DataGridCheckBoxColumn Header="UnHeader" Binding="{Binding UnElementDeFoo}" Width="*"/>
        </DataGrid.Columns>
    </DataGrid>

    <ListView x:Name="bar" ItemsSource="{Binding Bar, RelativeSource={RelativeSource FindAncestor, AncestorType={x:Type local:MainWindow}}}">
        <ListView.View>
            <GridView>
                <GridViewColumn/>
            </GridView>
        </ListView.View>
    </ListView>

    <!-- ... -->
    <!-- ... -->
    <!-- ... -->
</Grid>
```

## Code métier

### Dans MainWindow

```csharp
public ObservableCollection<object> Foo { get; set; } // Au lieu de object, mettre l'objet en question, par exemple : Client

public MainWindow()
{
    
    InitializeComponent();
    Foo = new ObservableCollection<object>(Data.Load()); // Au lieu de object, mettre l'objet en question, par exemple : Client
    bar.SelectedItem = LesClients[0];
}
```

## Style

### Triggers

On applique un style en fonction d'une valeur (dans l'exemple, un boolean)

```xml
<Window.Resources>
    <Style TargetType="{x:Type DataGridCell}">
        <Style.Triggers>
            <DataTrigger Binding="{Binding Path=Payee}" Value="False">
                <Setter Property="Background" Value="Red"/>
            </DataTrigger>
            <DataTrigger Binding="{Binding Path=Payee}" Value="True">
                <Setter Property="Background" Value="LightGreen"/>
            </DataTrigger>
        </Style.Triggers>
    </Style>
</Window.Resources>
```

### ControlTemplate 

On applique un validateur qui va vérfier la conforité d'une donnée et appliquer un style en conséquence.

On ajoute dans le TextBox : 

```xml
<TextBox Text="{Binding Nom, ValidatesOnExceptions=True}" Validation.ErrorTemplate="{StaticResource txtError}">
```

On créer la resource txtError :

```xml
<ControlTemplate x:Key="txtError">
    <StackPanel Orientation="Horizontal">
        <Border BorderBrush="Red" BorderThickness="1">
            <AdornedElementPlaceholder x:Name="element"/>
        </Border>
        <TextBlock x:Name="tbError" Foreground="Red" Margin="5,0,0,0" FontSize="10pt" Text="{Binding ElementName=element, Path=AdornedElement.(Validation.Errors)[0].ErrorContent}"/>
    </StackPanel>
</ControlTemplate>
```

