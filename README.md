# VisualBasic
Imports System.Globalization
Imports System.IO
Imports System.Linq.Expressions
Imports System.Net
Imports System.Reflection.Emit
Imports System.Reflection.Metadata.Ecma335

Imports System.Text.RegularExpressions
Imports System.Text.RegularExpressions.MatchCollection
Imports System.Windows.Forms.VisualStyles.VisualStyleElement


Public Class Form1
    Dim desktopPath As String = Environment.GetFolderPath(Environment.SpecialFolder.Desktop)
    Dim newfile As String = "Tejasri.txt"

    Dim newPath As String = System.IO.Path.Combine(desktopPath, newfile)




    Private Sub OpenFileDialog1_FileOk(sender As Object, e As System.ComponentModel.CancelEventArgs)

    End Sub

    Private Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        ' Hiding the TextBox
        TextBox1.Visible = True

        ' Call ShowDialog.
        Dim result As DialogResult = OpenFileDialog1.ShowDialog()
        TextBox1.ScrollBars = ScrollBars.Vertical

        ' Testing the result.
        If result = Windows.Forms.DialogResult.OK Then

            'Getting the file name.
            Dim path As String = OpenFileDialog1.FileName
            Try
                ' Reading the textfile and storing into textfile variable.
                'Dim textFile As String = File.ReadAllText(path)
                Dim pathofthefile As String = OpenFileDialog1.FileName  'Getting the File name
                Dim resultNames As New List(Of String)                  ' Declaring the List names resultNames to store the resulting names
                Dim Listelements As New ArrayList()
                Dim TextFile As String = File.ReadAllText(pathofthefile) ' Storing contents of the file to a variable named TextFile  

                Dim NamesArray1() As String = Split(TextFile, "info")   ' Splitting the TextFile at <name> tag '
                For index As Integer = 0 To NamesArray1.Length - 1
                    Dim contentsOfTextFile As String = NamesArray1(index)            'Storing the contents of NamesArray into contentsofTextFile variable

                    Dim NameString As String = "/Tag"

                    Dim searchResult As Integer = contentsOfTextFile.IndexOf(NameString)
                    Dim NamesArray2() As String = Split(NamesArray1(index), "/Tag") 'splitting the First list at </name> tag'
                    If searchResult = -1 Then

                    Else
                        Console.WriteLine(NamesArray2(0))
                        resultNames.Add(NamesArray2(0))       ' appending all the names into resultNames array '
                        If Listelements.Contains(NamesArray2(0)) Then

                        Else
                            Listelements.Add(NamesArray2(0))
                        End If
                    End If
                Next
                Console.WriteLine(resultNames)             'writing the resulting array contents into new file 
                Dim sResult As String = String.Join(" ", Listelements.ToArray())
                'sResult = ParseString(TextBox1.Text)
                Dim pattern As Regex = New Regex("[^A-Z a-z ,.\n]")
                If (pattern.IsMatch(sResult)) Then
                    Dim ResultRegex As MatchCollection = pattern.Matches(sResult)
                    TextBox1.Text = sResult.ToString


                    'TextBox2.Text = ResultRegex(0)
                End If


                Using sw As New System.IO.StreamWriter(newPath)

                    For Each item As String In Listelements
                        'sw.WriteLine((Listelements.IndexOf(item) + 1 & "," & item))
                        sw.Write(item)
                    Next
                    'ParseString(TextBox1.Text)
                    sw.Flush()
                    sw.Close()
                End Using
                reader()
                'ParseString(sResult)
                'I have refered this code form https://www.tutorialspoint.com/vb.net/vb.net_text_files.html'



                'TextBox1.Visible = True
                'TextBox1.Text = textFile.ToString

            Catch ex As Exception

                ' Assigning Error value in Text Box
                TextBox1.Text = "Error while loading Text"

            End Try

        End If

    End Sub
    Sub reader()               ' Reading the contents of resulting array 
        Using sr As New System.IO.StreamReader(newPath)
            Console.WriteLine(sr.ReadToEnd)
            sr.Close()
        End Using
        Console.ReadLine()
    End Sub
    Public Function ParseString(input As String) As String
        Dim pattern As String = "[^A-Za-z ,.\n]"
        Dim rgx As New Regex(pattern)
        Dim output As String = rgx.Replace(input, "")
        Return output
    End Function

    Private Sub Button2_Click(sender As Object, e As EventArgs) Handles Button2.Click
        Me.Close()
    End Sub
End Class
