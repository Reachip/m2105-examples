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
public ObservableCollection<object> LesClients { get; set; } // Au lieu de object, mettre l'objet en question, par exemple : Client

public MainWindow()
{
    
    InitializeComponent();
    Foo = new ObservableCollection<object>(Data.Load()); // Au lieu de object, mettre l'objet en question, par exemple : Client
    bar.SelectedItem = LesClients[0];
}
```