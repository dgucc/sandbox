# VBA tips

## Excel

Custom Function :  

```vba
Public Function checkDigitRRN(ByVal target As Range) As Integer
    Dim base
    base = CLng(Left(Trim(target.Value), 9))
    If (Len(target.Value) = 10) Then
        base = CLng("20" & Left(Trim(target.Value), 8))
    End If
    checkDigitRRN = 97 - base Mod 97
End Function
```

Write to File (unicode : UTF-19 LE BOM) :  

```vba
Sub writeToFile(fileName As String, strLine As String)
Dim outputFile As String
    outputFile = Replace(ThisWorkbook.FullName, ThisWorkbook.Name, "", 1, 1, vbTextCompare) & fileName
    Set fso = CreateObject("Scripting.FileSystemObject")
    Set out = fso.CreateTextFile(outputFile, True, True) ' filename, overwrite:true, unicode:true
    out.WriteLine strLine
    out.Close
End Sub
```

Write to Log :  
```vba
Sub log(strLine As String, Optional appendMode As Boolean = True)

Dim logFile As String
Dim strDate As String
Dim fileNumber As Integer

   strDate = Format(Date, "yyyymmdd")
   logFile = Replace(ThisWorkbook.FullName, ".xls", "_" & strDate & ".log", 1, 1, vbTextCompare)
   fileNumber = FreeFile
   
   If appendMode Then
      Open logFile For Append As #fileNumber
   Else
      Open logFile For Output As #fileNumber
   End If
   
   Print #fileNumber, strLine
   Close #fileNumber

End Sub
```

Highlight substring in cells :   
```vba
' Call highlightSubstringInCell("textToFind", Selection.Cells)

Sub highlightSubstringInCell(subString As String, selectedCells As Range, Optional startSearchAt As Integer = 1)

Dim objRange As Range
Dim start
Dim length As Integer

   For Each objRange In selectedCells.Cells   
      start = InStr(startSearchAt, objRange.Value, subString, vbTextCompare)
      
      Do            
         If (start > 0) Then
            With objRange.Characters(start:=start, length:=Len(subString)).Font
               .FontStyle = "Bold"
               .ColorIndex = 3 ' red
            End With
            start = InStr(start + 1, objRange.Value, subString, vbTextCompare)
         Else
            Exit Do
         End If                     
      Loop While (start > 0)
   
   Next ' For Each objRange In selectedCells.Cells
   
End Sub
```

Autoformat Rows Color - Alternate Per Group (ActiveCell) :  

```vba
Sub clearInteriorColor(skipHeaderRow As Boolean)
    Dim myRange As Range
    
    Set myRange = ActiveCell.CurrentRegion
    
    ' Skip the header row
    If skipHeaderRow Then
        Set myRange = myRange.Offset(1, 0) '.Resize(myRange.Rows.Count - 1, myRange.Columns.Count)
    End If
    
    myRange.Interior.ColorIndex = xlNone
    
    Set myRange = Nothing
End Sub
Sub alternateRowColorPerGroup()
'
' Alternate Rows Color Per Group
' taking the values of the activecell column as grouping key
'

Dim entireRange As Range
Dim currentRange As Range
Dim previous As Variant
Dim counter As Integer


On Error Resume Next
    Application.Calculation = xlCalculationManual
    Application.ScreenUpdating = False
       
    Set entireRange = ActiveCell.CurrentRegion
       
    ' Point the highest cell in the activeCell Column avoiding header row (rows(2))
    Set currentRange = Application.Intersect(entireRange.Rows(2), ActiveCell.EntireColumn)
       
    clearInteriorColor (True)
    
    previous = currentRange.Value
    counter = 0
    Do While (currentRange.Row <= entireRange.Rows.Count)
        If currentRange.Value <> previous Then
            counter = counter + 1
        End If
        If counter Mod 2 <> 0 Then
            With entireRange.Rows(currentRange.Row).Interior
                .ColorIndex = 15 ' light grey
                .Pattern = xlSolid
            End With
            'entireRange.Rows(currentRange.Row).Font.Bold = True
        End If
        previous = currentRange.Value
        Set currentRange = currentRange.Offset(1, 0)
    Loop
    
    Set entireRange = Nothing
    Set currentRange = Nothing
    Set previous = Nothing
    
    Application.Calculation = xlCalculationAutomatic
    Application.ScreenUpdating = True

End Sub
```

List files :  

```vba
Sub getFilesList(fileFilter As String, folderToScan As String, scanInSubFolder As Boolean, ByVal resultsRange As Range)
' Call getFilesList("*.csv", ThisWorkbook.Path & "files\", False, ThisWorkbook.Sheets(1).Range("B2"))

Dim fs As FileSearch
Dim i As Integer
Dim currCell As Range
Dim sheet As Worksheet

On Error Resume Next
    Application.Cursor = xlWait
           
   Set sheet = resultsRange.Worksheet
   sheet.Cells.ClearContents
   'sheet.Cells.Offset(1, 0).ClearFormats
   sheet.Cells.ClearFormats
   
    Set fs = Application.FileSearch
        
    fs.LookIn = folderToScan
    fs.Filename = fileFilter
    ' Recursive Search ?
    fs.SearchSubFolders = scanInSubFolder
    fs.MatchTextExactly = False
    fs.FileType = msoFileTypeAllFiles
    
    If fs.Execute(SortBy:=msoSortByFileName, SortOrder:=msoSortOrderAscending) > 0 Then
        Set currCell = resultsRange.Range("A1")
        ' for each file found
        For i = 1 To fs.FoundFiles.Count
            currCell.Value = fs.FoundFiles(i)
            Set currCell = currCell.Offset(1, 0)
        Next i        
    End If
        
    Set fs = Nothing
    
    Application.Cursor = xlDefault    
End Sub
```

XslTransform :  

```vba
' Add Reference to Microsoft XML, v6.0

Sub main()
Dim cell As Range
Dim sourceFile As String
Dim xslFile As String
Dim exportFile As String
Dim filePath As String
   
Debug.Print "*** Start : " & Now() & " ***"
   
filePath = Application.ThisWorkbook.Path & "\"

For Each cell In ThisWorkbook.Sheets("Sheet1").UsedRange.Columns(1).Offset(1, 0).Cells
   If Trim(cell.Value) <> "" Then
      sourceFile = filePath & Trim(cell.Offset(0, 0).Value)
      xslFile = filePath & Trim(cell.Offset(0, 1).Value)
      exportFile = filePath & Trim(cell.Offset(0, 2).Value)
   
      Call xslTransform(sourceFile, xslFile, exportFile, cell.Offset(0, 3))
      Debug.Print exportFile
   End If
Next   
   
Debug.Print "*** Terminated : " & Now() & " ***"
End Sub

Sub xslTransform(sourceFile As String, stylesheetFile As String, resultFile As String, errorCell As Range)

Dim source As New MSXML2.DOMDocument30
Dim stylesheet As New MSXML2.DOMDocument30
Dim result As New MSXML2.DOMDocument30

' Load data.
source.async = False
source.Load sourceFile

' Load style sheet.
stylesheet.async = False
stylesheet.Load stylesheetFile


If (source.parseError.ErrorCode <> 0) Then
   MsgBox ("Error loading source document: " & source.parseError.reason)
   Else
   If (stylesheet.parseError.ErrorCode <> 0) Then
         'MsgBox ("Error loading stylesheet document: " & stylesheet.parseError.reason)
      Debug.Print Trim("Error loading stylesheet document: [" & stylesheet.parseError.ErrorCode & "] " & stylesheet.parseError.reason)
      errorCell.Value = Trim("Error loading stylesheet document: [" & stylesheet.parseError.ErrorCode & "] " & stylesheet.parseError.reason)
   Else
      ' Do the transform.
      source.transformNodeToObject stylesheet, result
      result.Save resultFile
      errorCell.Value = ""
   End If
End If

End Sub
```
Random Numbers :  

```vba
Sub main_test()
Dim i As Integer

For i = 1 To 15
    Debug.Print RandomNumbers(0, 500, 2)
Next i
    
End Sub
Sub randomInt()

    Randomize
   
    'Random whole number between 1 and 50 :
    random_number = Int(50 * Rnd) + 1
   
    MsgBox random_number
   
End Sub

Public Function RandomNumbers(Lowest As Long, Highest As Long, Optional Decimals As Integer)
   Application.Volatile  'Remove this line to "freeze" the numbers
   If IsMissing(Decimals) Or Decimals = 0 Then
      Randomize
      RandomNumbers = Int((Highest + 1 - Lowest) * Rnd + Lowest)
   Else
      Randomize
      RandomNumbers = Round((Highest - Lowest) * Rnd + Lowest, Decimals)
   End If
End Function
```

ODBC sample :   

```vba
Sub getDBData(ByVal sqlCommand As String, tableName As String)
Const adFldIsNullable = 32

'Add Reference to Microsoft ActiveX Data Objects 2.0 Library
Dim objConnection As ADODB.Connection
Dim objRecordset As ADODB.Recordset
Dim objCommand As ADODB.Command
Dim sqlStatement As String

Dim i As Integer
Dim iRow As Integer
Dim myDate As String
Dim cell As Range

On Error GoTo errDateHandler

Application.ScreenUpdating = False
Application.Calculation = xlCalculationManual

    ' Open dbConnection
    Set objConnection = New ADODB.Connection
    objConnection.Open "Provider=OraOLEDB.Oracle.1;Password=Password;Persist Security Info=True;User ID=admin;Data Source=myDataSource;"
    
    ' Prepare sqlStatement
    Set objCommand = New ADODB.Command
    Set objCommand.ActiveConnection = objConnection
    objCommand.CommandType = adCmdText
    objCommand.CommandText = sqlCommand
    
    ' Load Recordset
    Set objRecordset = objCommand.Execute
        
    
    If ThisWorkbook.Sheets("tablesADO").Range("A2").Value <> "" Then
        Set cell = ThisWorkbook.Sheets("tablesADO").Range("A1").End(xlDown).Offset(1, 0)
    Else
        Set cell = ThisWorkbook.Sheets("tablesADO").Range("A2")
    End If
    
    For i = 0 To objRecordset.Fields.Count - 1
        cell.Offset(i, 0).Value = tableName
        cell.Offset(i, 1).Value = objRecordset.Fields(i).Name
        Select Case objRecordset.Fields(i).Type
            Case 131
                cell.Offset(i, 2).Value = "adNumeric"
            Case 135
                cell.Offset(i, 2).Value = "adDBTimeStamp"
            Case Else
                cell.Offset(i, 2).Value = "adVarChar"
        End Select
        cell.Offset(i, 3).Value = objRecordset.Fields(i).Precision
        cell.Offset(i, 4).Value = objRecordset.Fields(i).Attributes
        cell.Offset(i, 4).Value = objRecordset.Fields(i).Attributes
        If ((adFldIsNullable And objRecordset.Fields(i).Attributes) = adFldIsNullable) Then
            cell.Offset(i, 5).Value = ""
        Else
            cell.Offset(i, 5).Value = "NOT NULL"
        End If

    Next i    

objRecordset.Close
Set objRecordset = Nothing

objConnection.Close
Set objConnection = Nothing
    
    
finalExit:
    
Application.ScreenUpdating = True
Application.Calculation = xlCalculationAutomatic

ThisWorkbook.Sheets("tablesADO").Activate


'MsgBox ("Terminated")
Exit Sub

errDateHandler:
Debug.Print Err.Description & " [" & objCommand.CommandText & "]"
MsgBox (Err.Description)
GoTo finalExit
'Resume Next


End Sub
```
