# Powershell

In this lab I will try to show some basic Powersell exercises.

First of all, we need to create a CSV file https://www.convertcsv.com/generate-test-data.htm

If you want to do this from Windows 10 or 11 client machine then you need to  download RSAT Tools from Microsoft website  https://learn.microsoft.com/en-us/troubleshoot/windows-server/system-management-components/remote-server-administration-tools

Note: Windows Client Machine should be connected to the domain.

You can put the list of employees file into notepad then save it to C: disk. 

Make sure that user is under your domain name ,not in a folder to find the user

You can also activate Advanced Features to see more options about user View > Advanced Features

![Screenshot 2025-05-24 183122](https://github.com/user-attachments/assets/65b17eeb-6874-42e9-ae87-f3194989e549)
![Screenshot 2025-05-24 183911](https://github.com/user-attachments/assets/0054fca1-50ad-420f-92f6-6b6076e36c62)
![Screenshot 2025-05-24 185825](https://github.com/user-attachments/assets/a6cdc1c5-34ce-42f0-94dc-7c1b08f97de7)

#load in the CSV file for employees

We are going to use this: 
   
        -Import-Csv -Path "C:\Employees.csv" -Delimiter "," 

Most people stop here so instead we continue our command line. To write name and Last name properly and we can change for example, "office" to "physicalDeliveryOfficeName" or "FirstName" to " GivenName 

instead of changing later we cahnge now amd it will help us later

Simpflifying the Data

for example; @{Name="EmployeeID";Expression={$_.EmployeeID}}.GetType()  this code will give hashtable

      {$_.EmployeeID}.GetType()  

This code will give scriptblock

Creating Hashtables

 A hashtable is a data structure that stores key-value pairs. Creating a hashtable allows you to quickly look up values based on a key.
 
Why use hashtables?

  -Fast lookups by key.
  -Useful for storing and managing related data.
  -Flexible to add or remove keys and values at runtime.


 Enumerating hashtables

       $SyncProperties+$SyncFieldMap.GetEnumerator()

 ![Screenshot 2025-06-03 050156](https://github.com/user-attachments/assets/3c76e54b-a1ff-4820-96a3-6a4a1e1ebaab)

 Sync Properties


        $Properties=ForEach($Property in $SyncProperties){
             @{Name=$Property.Value}
        }      

Highlight GetEnumerator line to the buttom curly bracket run it then highlight $Properties only then run again you will have this 

![Screenshot 2025-06-04 053954](https://github.com/user-attachments/assets/a0545c70-b6c6-4bfb-ba40-91cd0928a1cb)


This is actually going to store array, our array as we know is going to be a bunch of a different hash tables and scriptblocks. Now we highlight and run from GetEnumurator then we highlight and run $Properties line we should have this 

![image](https://github.com/user-attachments/assets/d988d99f-d455-4b0a-8b04-24c222b7eac9)


we run this line and we should have this finally

      Import-Csv -Path "C:\Employees.csv" -Delimiter "," | Select-Object -Property $Properties

![Screenshot 2025-06-09 051512](https://github.com/user-attachments/assets/7b87615e-c24e-461f-b4e8-e4f72154fd76)


Next, we create function and we also put an error handling if user run into an error they can see a message 

   function Get-EmployeeFromCsv{
       [CmdletBinding()]
       Param(
            [Parameter(Mandatory)]
            [string]$FilePath,
            [Parameter(Mandatory)]
            [string]$Delimiter,
            [Parameter(Mandatory)]
            [hashtable]$SyncFieldMap
        )

      try{

           
