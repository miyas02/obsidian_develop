---

---
# Workbook, Worksheet

### **WorkbookとWorksheetの取得**

```vb.net
Dim wb As Workbook
Dim ws As Worksheet
Set ws = ThisWorkbook.Sheets("sheet1") ' シート名を指定
Set ws = ThisWorkbook.ActiveSheet ' アクティブシートを指定
```

### 新しいSheetを作成

```vb.net
' 新しいシートを作成（最後尾に追加）
Dim newSheet As Worksheet
Set newSheet = ThisWorkbook.Sheets.Add(After:=ThisWorkbook.Sheets(ThisWorkbook.Sheets.Count))
newSheet.Name = "新規シート名"
```

### シートの存在チェック

```vb.net
Function SheetExists(sName As String) As Boolean
    Dim ws As Worksheet
    On Error Resume Next
    Set ws = ThisWorkbook.Sheets(sName)
    On Error GoTo 0
    SheetExists = Not ws Is Nothing
End Function

' 呼び出し側
Dim oldSheet As Worksheet

If SheetExists("CSV取込") Then
    Set oldSheet = ThisWorkbook.Sheets("CSV取込")
    Application.DisplayAlerts = False ' 削除確認ダイアログを非表示
    oldSheet.Delete
    Application.DisplayAlerts = True
End If
```

# Cell

### Cellに値を入力
```vb.net
sheet.Cells(1, 2) = "value" 'sheet.Cells(row, column) 
```
### target_cellの隣のcellを取得 (Offset)
```

targetCell.Offset(0, -1).Value
```
# Row

### 特定の列を取得

```c#
Dim targetCol As Range
Set targetCol = Columns("C")  ' C列全体を取得
```

### Range

```c#

```

# 基本構文

### **最終行の取得, 最終列の取得**

```vb.net
Dim lastRow As Long
lastRow = ws.Cells(ws.Rows.Count, "B").End(xlUp).Row
```

```vb.net
Dim lastColumn As Long
lastColumn = ws.Cells(1, ws.Columns.Count).End(xlToLeft).Column
```

### **for文**

```vb.net
    Dim i As Long
    
    For i = 1 To lastRow ' lastRow定義済み前提
	    
    Next i
```

### **Dictionary**

```vb.net
' Dictionaryオブジェクトの作成
Dim dict As Object
Set dict = CreateObject("Scripting.Dictionary")

' キーと値の追加
dict.Add "key1", "value1"
dict.Add "key2", "value2"

' キーによる値の取得
Dim value As String
value = dict("key1") ' "value1"が返されます

' キーの存在チェック
If dict.Exists("key1") Then
    MsgBox "キーは存在します。"
Else
    MsgBox "キーは存在しません。"
End If
```

### 配列

**静的配列**

```vb.net
' 基本的な配列の宣言
Dim 変数名(インデックス) As データ型
' 例：10個の整数を格納できる配列
Dim numbers(9) As Long

' 配列へのアクセス
numbers(1) = 100
' 要素の値を取得して表示
Debug.Print numbers(1)
```

**動的配列**

```vb.net
' 動的配列の宣言（要素数は指定しない）
Dim numbers() As Long

' ReDim ステートメントを使用して、動的配列のサイズを設定したり変更したりできる
ReDim numbers(1 To 3)

' 配列のサイズを変更（既存の値は消去される）
' numbers(1)以降すべて0
ReDim numbers(1 To 10)

' Preserveステートメントを使用すると、既存の値を保持したままサイズを変更できる
ReDim Preserve numbers(1 To 10)
```

### リスト

```vb.net
    Dim arr As Object
    Set arr = CreateObject("System.Collections.ArrayList")
    arr.Add ("要素を追加")
```

### Collection

```vb.net
Dim col As Collection
Set col = New Collection
```

### 文字列の部分一致

```vb.net
Dim txt As String
txt = "スーパーで買い物"
If InStr(txt, "スーパー") > 0 Then
    MsgBox "部分一致しました"
End If
```

# デバッグ

```vb.net
debug.Assert i <> 10 ' デバッグ実行時、条件がFalseのとき停止する(iが10のとき停止)
If i = 10 Then Stop ' 明示的にデバッグを停止する
? ws.Cells(1, 2).Value ' イミディエイトウィンドウで変数の中身を確認
```

# VBA入門

### SubとFunctionの違い

- **Sub**
    - マクロとして実行可能
    - 戻り値なし
- **Function**
    - マクロとして実行不可
    - 戻り値あり

**Functionの構文**

```vb.net
[Private or Public] Function 関数名([引数リスト]) [As 戻り値の型]
' 戻り値の設定
    関数名 = 戻り値
End Function
```

### ノットイコール

```vb.net
If x <> 5 Then
	MsgBox "xは5ではありません"
End If
```