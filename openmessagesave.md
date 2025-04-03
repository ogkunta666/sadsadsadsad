# Save - OpenFileDialog + MessageBox

# Feladat leírása 

Egy menthető és betölthető TextBox ahol MenuStrip-ben lehet kiválasztani hogy mit szeretnél csinálni a fájllal , és a kilépés előtt egy megerősítést kér a felhasználótól.

# Grid  felépítése : 

Egy Menu benne MenuItemekkel aminél különböző metódusok vannak meghívva kattintás esetén a gombhoz megfelelően , és egy TextBox hogy valamit lehessen is csinálni.

```c#
<Grid>
        <Menu VerticalAlignment="Top">
            <MenuItem Header="Fájl">
                <MenuItem Header="Megnyitás" Click="OpenFileMenu_Click"/>
                <MenuItem Header="Mentés" Click="SaveFileMenu_Click"/>
                <MenuItem Header="Kilépés" Click="ExitMenu_Click"/>
            </MenuItem>
        </Menu>
        <TextBox Name="TextTextBox" AcceptsReturn="True" VerticalScrollBarVisibility="Auto" Margin="10"/>
    </Grid>
```

A textboxhoz semmilyen egyéb dolgunk nem akad.

# OpenFileDialog 

A fájl megnyitásához egy új OpenFileDialog változó típusú objektumot kell felvennünk amit filterezünk hogy csak .txt fájlokat adjon ki megnyitásnál de ez lehet
program függően bármi más (.exe , .bat , .msi , .docx). Megnyitást követően a TextTextBox tartalmát átírjuk a megnyitott .txt fájl összes tartalmára. 

```c#
private void OpenFile_Click(object sender, RoutedEventArgs e)
        {
            OpenFileDialog openFileDialog = new OpenFileDialog
            {
                Filter = "Szövegfájlok (*.txt)|*.txt"
            };
            if (openFileDialog.ShowDialog() == true)
            {
                TextTextBox.Text = File.ReadAllText(openFileDialog.FileName);
            }
        }
```

Több fájlt is megtudunk nyitni hogyha a filter után engedélyezzük ezt.

```c#
Multiselect = true
```

# SaveFileDialog

Az előzővel ellentettesen ez a fájlok mentésére való ugyan úgy kell filterezni annyi különbséggel hogy itt írjuk és nem olvassuk a szöveget.

```c#
 private void SaveFile_Click(object sender, RoutedEventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog
            {
                Filter = "Szövegfájlok (*.txt)|*.txt"
            };
            if (saveFileDialog.ShowDialog() == true)
            {
                File.WriteAllText(saveFileDialog.FileName, TextTextBox.Text);
                MessageBox.Show("Fájl sikeresen mentve!", "Mentés", MessageBoxButton.OK);
            }
        }
```

# MessageBox

Itt ami még érdekesség lehet az a MessageBox vessző utáni része. 

```c#
"Mentés"
```

Ez a mentés a MessageBox headerjére vonatkozik szóval amit ide adunk meg az lesz a MessageBox ablakának a neve ez lehet bármi.

```c#
MessageBoxButton.OK 
```

Ez pedig azt éri el hogy kiadjon egy "OK" gombot hogy a felhasználó mindenféleképpen interaktáljon a programmal ez ebben a kódban nem szükséges
de más fájlokon alapuló programokban vagy nagyobb hibaüzenet esetén ez kifejezetten fontos.

# Result

Ha a felhasználó kiakar lépni akkor megerősítésre várunk.
Erre nagy segítség a MessageBoxResult ami pedig vagy igen vagy nem lehet. Igen kattintás esetén a program leáll ha nem akkor minden megy tovább.
Az ablakban egy Warning kép is megjelenik még.
```c#
 private void Exit_Click(object sender, RoutedEventArgs e)
        {
            MessageBoxResult result = MessageBox.Show("Biztosan ki akar lépni?", "Kilépés", MessageBoxButton.YesNo, MessageBoxImage.Warning);
            if (result == MessageBoxResult.Yes)
            {
                Application.Current.Shutdown();
            }
        }
```

Az ifben lévő rész olyan mintha az AltF4-et nyomnánk meg mert az bezárja a perpillanat futó alkalmazást.

# A MessageBox elérhető paraméterjei : 

 1. elem : szöveg , ami megjelenik a messagebox-ban
 2. elem : szöveg , ami a messagebox címe
 3. elem : gombok amik az alábbiak lehetnek : OK , OKCancel , YesNo , YesNoCancel

Ikonok : 
1. ikon : Information , információs ikon jelenik meg
2. ikon : Warning figyelmeztető ikon jelenik meg
3. ikon : Error , hiba ikon jelenik meg
4. ikon : Question egy kérdőjel jelenik meg

A MessageBoxButton.YesNo hozzá ad a messageboxhoz kettő gombot ami vagy igen vagy nem ez azért fontos hogy a resultot áttudjuk adni az ifnek és annak következménye 
történjen. Azonban alapértelmezett gombot is tudunk megadni ami az enter gomb megnyomására egyből a result lesz.

```c#
MessageBox.Show("Kilépsz az alkalmazásból?", "Kilépés", MessageBoxButton.YesNo, MessageBoxImage.Question, MessageBoxResult.No);
```




Az egész XAML kód : 

```c#
<Window x:Class="OpenSaveMessage.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="OpenSaveMessage" Height="450" Width="800">
    <Grid>
        <Menu VerticalAlignment="Top">
            <MenuItem Header="Fájl">
                <MenuItem Header="Megnyitás" Click="OpenFile_Click"/>
                <MenuItem Header="Mentés" Click="SaveFile_Click"/>
                <MenuItem Header="Kilépés" Click="Exit_Click"/>
            </MenuItem>
        </Menu>
        <TextBox Name="TextTextBox" VerticalScrollBarVisibility="Auto" Margin="10"/>
    </Grid>
</Window>
```


Az egész .xaml.cs kód : 

```c#
public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void OpenFile_Click(object sender, RoutedEventArgs e)
        {
            OpenFileDialog openFileDialog = new OpenFileDialog
            {
                Filter = "Szövegfájlok (*.txt)|*.txt"
            };
            if (openFileDialog.ShowDialog() == true)
            {
                TextTextBox.Text = File.ReadAllText(openFileDialog.FileName);
            }
        }

        private void SaveFile_Click(object sender, RoutedEventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog
            {
                Filter = "Szövegfájlok (*.txt)|*.txt"
            };
            if (saveFileDialog.ShowDialog() == true)
            {
                File.WriteAllText(saveFileDialog.FileName, TextTextBox.Text);
                MessageBox.Show("Fájl sikeresen mentve!", "Mentés", MessageBoxButton.OK, MessageBoxImage.Information);
            }
        }

        private void Exit_Click(object sender, RoutedEventArgs e)
        {
            MessageBoxResult result = MessageBox.Show("Biztosan ki akar lépni?", "Kilépés", MessageBoxButton.YesNo, MessageBoxImage.Warning);
            if (result == MessageBoxResult.Yes)
            {
                Application.Current.Shutdown();
            }
        }
    }
```

