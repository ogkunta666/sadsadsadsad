# Save - OpenFileDialog + MessageBox

# Feladat leírása 

Egy menthető és betölthető TextBox ahol MenuStrip-ben lehet kiválasztani hogy mit szeretnél csinálni a fájllal , és a kilépés előtt egy megerősítést kér a felhasználótól.

# Grid : 

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
Azonban ha a felhasználó máshova kattint a kelleténél akkor egy figyelmeztető hangot játszik le. Ehhez szükséges az is hogy a program elejére
felvegyük a "using Microsoft.Win32"-t.

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

A MessageBoxButton.YesNo hozzá ad a messageboxhoz kettő gombot ami vagy igen vagy nem ez azért fontos hogy a resultot áttudjuk adni az ifnek és annak következménye 
történjen.

Az ifben lévő rész pedig olyan mintha az AltF4-et nyomnánk meg mert az bezárja a perpillanat futó alkalmazást.