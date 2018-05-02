---
title: "Recordset.Sort Property (DAO)"
 
 
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
ms.topic: reference
f1_keywords:
- dao360.chm1053063
  
localization_priority: Normal
ms.assetid: 9be9bf62-f713-537e-4df0-3a54d485a523
description: "Sets or returns the sort order for records in a Recordset object (Microsoft Access workspaces only)."
---

# Recordset.Sort Property (DAO)

Sets or returns the sort order for records in a **[Recordset](recordset-object-dao.md)** object (Microsoft Access workspaces only). 
  
## Syntax

 *expression*  . **Sort**
  
 *expression*  A variable that represents a **Recordset** object. 
  
## Remarks

You can use the **Sort** property with dynaset- and snapshot-type **Recordset** objects. 
  
When you set this property for an object, sorting occurs when a subsequent **Recordset** object is created from that object. The **Sort** property setting overrides any sort order specified for a **[QueryDef](querydef-object-dao.md)** object. 
  
The default sort order is ascending (A to Z or 0 to 100).
  
The **Sort** property doesn't apply to table- or forward-only-type **Recordset** objects. To sort a table-type **Recordset** object, use the **[Index](recordset-index-property-dao.md)** property. 
  
> [!NOTE]
> In many cases, it's faster to open a new **Recordset** object by using an SQL statement that includes the sorting criteria. 
  
## Example

This example demonstrates the **Sort** property by changing its value and creating a new **Recordset**. The SortOutput function is required for this procedure to run. 
  
```
Sub SortX() 
 
 Dim dbsNorthwind As Database 
 Dim rstEmployees As Recordset 
 Dim rstSortEmployees As Recordset 
 
 Set dbsNorthwind = OpenDatabase("Northwind.mdb") 
 Set rstEmployees = _ 
 dbsNorthwind.OpenRecordset("Employees", _ 
 dbOpenDynaset) 
 
 With rstEmployees 
 SortOutput "Original Recordset:", rstEmployees 
 .Sort = "LastName, FirstName" 
 ' Print report showing Sort property and record order. 
 SortOutput _ 
 "Recordset after changing Sort property:", _ 
 rstEmployees 
 ' Open new Recordset from current one. 
 Set rstSortEmployees = .OpenRecordset 
 ' Print report showing Sort property and record order. 
 SortOutput "New Recordset:", rstSortEmployees 
 rstSortEmployees.Close 
 .Close 
 End With 
 
 dbsNorthwind.Close 
 
End Sub 
 
Function SortOutput(strTemp As String, _ 
 rstTemp As Recordset) 
 
 With rstTemp 
 Debug.Print strTemp 
 Debug.Print " Sort = " &amp; _ 
 IIf(.Sort <> "", .Sort, "[Empty]") 
 .MoveFirst 
 
 ' Enumerate Recordset. 
 Do While Not .EOF 
 Debug.Print " " &amp; !LastName &amp; _ 
 ", " &amp; !FirstName 
 .MoveNext 
 Loop 
 
 End With 
 
End Function 

```

When you know the data you want to select, it's usually more efficient to create a **Recordset** with an SQL statement. This example shows how you can create just one **Recordset** and obtain the same results as in the preceding example. 
  
```
Sub SortX2() 
 
 Dim dbsNorthwind As Database 
 Dim rstEmployees As Recordset 
 
 Set dbsNorthwind = OpenDatabase("Northwind.mdb") 
 ' Open a Recordset from an SQL statement that specifies a 
 ' sort order. 
 Set rstEmployees = _ 
 dbsNorthwind.OpenRecordset("SELECT * " &amp; _ 
 "FROM Employees ORDER BY LastName, FirstName", _ 
 dbOpenDynaset) 
 
 dbsNorthwind.Close 
 
End Sub 

```

