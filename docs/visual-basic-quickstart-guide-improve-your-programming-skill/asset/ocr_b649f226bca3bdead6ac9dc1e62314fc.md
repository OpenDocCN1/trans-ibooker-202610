15. grades (UBound(grades)) = grade: This line assigns the new grade variable to the last position in the grades array.

16. studentGrades (studentName) = grades: This updates the dictionary entry for studentName with the updated grades array.

17. Loop: This is the end of the loop. It continues until the end of the input file is reached.

18. inputFile.Close: This line closes the input file.

19. For Each studentName In studentGrades: This loop iterates over each student name in the studentGrades dictionary.

20. grades = studentGrades(studentName): This line retrieves the list of grades associated with the current studentName.

21. gradeList = Join(grades, ", "): This line joins the grades array elements into a string, separated by a comma and a space.

22. WScript.Echo studentName & ": " & gradeList: This line outputs the student name and their corresponding grade list to the console.

Overall, this code reads the CSV file, stores the student names and grades in a dictionary, and then prints the student names, along with their respective grades:

```
Set fso = CreateObject("Scripting.FileSystemObject")
Set inputFile = fso.OpenTextFile("students.csv", 1, False)
Dim studentGrades
Set studentGrades = CreateObject("Scripting.Dictionary")
Do Until inputFile.AtEndOfStream
  line = inputFile.ReadLine
  fields = Split(line, ",")
  If UBound(fields) = 1 Then
    studentName = Trim(fields(0))
    grade = Trim(fields(1))
    If Not studentGrades.Exists(studentName) Then
      studentGrades.Add studentName, Array(grade)
    Else
      grades = studentGrades(studentName)
      ReDim Preserve grades(UBound(grades) + 1)
      grades(UBound(grades)) = grade
      studentGrades(studentName) = grades
    End If
  End If
Loop
inputFile.Close
For Each studentName In studentGrades
  grades = studentGrades(studentName)
  gradeList = Join(grades, ", ")
  WScript.Echo studentName & ": " & gradeList
Next
```

# Coding the VBScript grading tool with Notepad

The following steps show you how to code and execute the previous VBScript, which runs a calculator program:

+   1. Start Notepad.
+   2. Enter the code from the preceding section.