---

---
### App.xaml

WPFアプリを実行したとき、**最初に呼ばれるのは **`**App**`** クラスの **`**OnStartup()**`** メソッド**

コードビハインド
「**XAML（見た目）と紐づいて動作を書く C# コード部分**」のこと

**App.xaml**
アプリケーション全体の設定やリソースを定義し、`StartupUri`で最初に開くウィンドウを指定

```c#
<Application x:Class="YourNamespace.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             StartupUri="MainWindow.xaml">
</Application>
```

**App.xaml.cs**

`App`クラスのコードビハインド。
`OnStartup`などのイベントをオーバーライドできる

```c#
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
        // アプリ起動時の処理
    }
}
```

# コードビハインド

### イベントハンドラ

| 引数 | 型 | 内容 |
| --- | --- | --- |
| `sender` | `object` | イベントを発生させたオブジェクト（この場合はButton） |
| `e` | `RoutedEventArgs` | イベントに関する情報（どのUI要素から発生したかなど） |

```c#
//xamlで次のように紐づけられている
//<Button Content="クリックしてね" Click="Button_Click"/>
private void Button_Click(object sender, RoutedEventArgs e) {
    MessageBox.Show("Hello, World!");
}
```

### INotifyPropertyChanged

プロパティの値が変わったことをUI（画面）に知らせるInterface

eventメンバにPropertyChangedEventHandlerデリゲードを定義

```c#
//interface定義(標準実装)
public interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler? PropertyChanged;
}

//実際の実行サンプル
using System.ComponentModel;
public class PersonViewModel : INotifyPropertyChanged{
    private string name;
    public string Name
    {
        get {return name;} //get {return name;}
        set
        {
            if (name != value)
            {
                name = value;
                OnPropertyChanged(nameof(Name)); //invokeを呼んでるだけ
            }
        }
    }
		//event
    public event PropertyChangedEventHandler? PropertyChanged;
		//メソッドの共通化
    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}

```

# XAML

## XAMLライブラリ

<Button>

### DataGrid

- **AutoGeneratingColumn**
`DataGrid` が列を自動生成する度に実行し、生成される各列をその場でカスタマイズ・キャンセルできる仕組み。
```xml
        <DataGrid
            AutoGenerateColumns="True" <!-- True --
            AutoGeneratingColumn="ItemsDataGrid_AutoGeneratingColumn"/>
```
```c#
        private void ItemsDataGrid_AutoGeneratingColumn(object sender, DataGridAutoGeneratingColumnEventArgs e) {
            e.Column.Width = new DataGridLength(200); //例 生成されたカラムの幅を指定
        }
```

<TextBlock> 標準的な文字列表示

### Grid

レイアウトコンテナ

```xml
<Grid>
	<Grid.ColumnDefinitions>
	    <ColumnDefinition Width="Auto"/> <!-- Column1をAuto -->
	    <ColumnDefinition Width="*"/> <!-- Column2 -->
	</Grid.ColumnDefinitions>
	<xxx Grid.Column="0"/>
	<xxx Grid.Column="1"/>
</Grid>
```

<Grid> レイアウトコンテナ

### <Window>タグ

```xml
<Window x:Class="matome_phase1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:matome_phase1"
        mc:Ignorable="d"
        Title="MainWindow" Height="900" Width="1000"
        WindowState="Maximized"> <!-- ウィンドウ最大化 -->
```

## プロパティ

### HorizontalAlignment / VerticalAlignment

- 水平方向（左右）の配置方法

`Left`, `Center`, `Right`, `Stretch`

- 垂直方向（上下）の配置方法

`Top`, `Center`, `Bottom`, `Stretch`

### Height/Width

- 高さ/幅

`数値`または`Auto`

### Margin

`左,上,右,下`（例：`10,20,0,0`）
