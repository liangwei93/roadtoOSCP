
Location of a file

Get-ChildItem -Path C:\ -Include *interesting-file.txt* -File -Recurse -ErrorAction SilentlyContinue

2. Specify the contents of this file

Get-Content "C:\Program Files\interesting-file.txt.txt"


Base64 decode the file b64.txt on Windows. 




$path = 'C:\Users\Administrator\Desktop\emails\*'
$magic_word = 'password'
$exec = Get-ChildItem $path -recurse | Select-String -pattern $magic_word
echo $exec