---
title: LINQ to SQL-Übersetzung in Azure Cosmos DB
description: Informieren Sie sich über die unterstützten LINQ-Operatoren und die Zuordnung von LINQ-Abfragen zu SQL-Abfragen in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 3f8753518e1d54ddba4fc15a5a030308d0c112a1
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042491"
---
# <a name="linq-to-sql-translation"></a>LINQ zu SQL-Übersetzung

Der Azure Cosmos DB-Abfrageanbieter führt eine bestmögliche Zuordnung von LINQ-Abfragen in SQL-Abfragen in Cosmos DB durch. Wenn Sie die SQL-Abfrage abrufen möchten, die in LINQ übersetzt wird, verwenden Sie die `ToString()`-Methode für das generierte `IQueryable`-Objekt. Bei der folgenden Beschreibung wird davon ausgegangen, dass Sie mit den Grundsätzen von LINQ vertraut sind.

Das Typsystem der Abfrageanbieter unterstützt nur die primitiven JSON-Typen: numerisch, boolesch, Zeichenfolge und Null.

Der Abfrageanbieter unterstützt die folgenden skalaren Ausdrücke:

- Konstante Werte, einschließlich konstanter Werte der primitiven Datentypen zum Auswertungszeitpunkt der Abfrage.
  
- Indexausdrücke auf Eigenschaften/Arrays, die sich auf die Eigenschaft eines Objekts oder eines Arrayelements beziehen. Beispiel:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Arithmetische Ausdrücke, einschließlich allgemeiner arithmetischer Ausdrücke auf numerische und boolesche Werte. Eine vollständige Liste finden Sie in der [Azure Cosmos DB-SQL-Spezifikation](https://go.microsoft.com/fwlink/p/?LinkID=510612).
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Vergleichsausdrücke für Zeichenfolgen, einschließlich Vergleiche von Zeichenfolgenwerten mit konstanten Zeichenfolgen.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Erstellungsausdrücke für Objekte/Arrays, die ein Objekt mit einem Verbundwerttyp oder anonymen Typ bzw. ein Array solcher Objekte zurückgeben. Diese Werte können geschachtelt werden.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

## <a name="supported-linq-operators"></a><a id="SupportedLinqOperators"></a>Unterstützte LINQ-Operatoren

Der im SQL .NET SDK enthaltene LINQ-Anbieter unterstützt die folgenden Operatoren:

- **Select**: Projektionen, einschließlich Objektkonstruktion, werden in SQL SELECT übersetzt.
- **Where**: Filter werden in SQL WHERE übersetzt und unterstützen die Übersetzung von `&&`, `||` und `!` in die SQL-Operatoren.
- **SelectMany**: Ermöglicht das Entladen von Arrays in die SQL-JOIN-Klausel. Wird zum Verketten oder Verschachteln von Ausdrücken zum Filtern von Arrayelementen verwendet.
- **OrderBy** und **OrderByDescending**: Übersetzung in ORDER BY mit ASC oder DESC.
- Die Operatoren **Count**, **Sum**, **Min**, **Max** und **Average** für die Aggregation und deren asynchrone Entsprechungen **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync** und **AverageAsync**.
- **CompareTo**: Übersetzung in Bereichsvergleiche. Wird häufig für Zeichenfolgen verwendet, da sie nicht in .NET vergleichbar sind.
- **Skip** und **Take**: Übersetzung in SQL OFFSET und LIMIT zum Einschränken von Ergebnissen einer Abfrage und zum Durchführen der Paginierung.
- **Mathematische Funktionen**: Unterstützt die Übersetzung von `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan` und `Truncate` aus .NET in die entsprechenden integrierten SQL-Funktionen.
- **Zeichenfolgenfunktionen**: Unterstützt die Übersetzung von `Concat`, `Contains`, `Count`, `EndsWith`,`IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd` und `TrimStart` aus .NET in die entsprechenden integrierten SQL-Funktionen.
- **Arrayfunktionen**: Unterstützt die Übersetzung von `Concat`, `Contains` und `Count` aus .NET in die entsprechenden integrierten SQL-Funktionen.
- **Geospatial-Erweiterungsfunktionen**: Unterstützt die Übersetzung aus den Stubmethoden `Distance`, `IsValid`, `IsValidDetailed` und `Within` in die entsprechenden integrierten SQL-Funktionen.
- **Erweiterungsfunktion für benutzerdefinierte Funktion**: Unterstützt die Übersetzung aus der Stubmethode `UserDefinedFunctionProvider.Invoke` in die entsprechende benutzerdefinierte Funktion.
- **Sonstiges**: Unterstützt die Übersetzung von `Coalesce` und bedingten Operatoren. Kann `Contains` je nach Kontext in die Zeichenfolge CONTAINS, ARRAY_CONTAINS oder SQL IN übersetzen.

## <a name="examples"></a>Beispiele

Die folgenden Beispiele zeigen, wie einige der Standard-LINQ-Abfrageoperatoren in Cosmos DB-Abfragen übersetzt werden.

### <a name="select-operator"></a>Select-Operator

Die Syntax ist `input.Select(x => f(x))`, wobei `f` ein skalarer Ausdruck ist.

**Select-Operator, Beispiel 1:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Select-Operator, Beispiel 2:** 

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Select-Operator, Beispiel 3:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

### <a name="selectmany-operator"></a>SelectMany-Operator

Die Syntax lautet `input.SelectMany(x => f(x))`, wobei `f` ein skalarer Ausdruck ist, der einen Containertyp zurückgibt.

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>Where-Operator

Die Syntax ist `input.Where(x => f(x))`, wobei `f` ein skalarer Ausdruck ist, der einen booleschen Wert zurückgibt.

**Where-Operator, Beispiel 1:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**Where-Operator, Beispiel 2:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

## <a name="composite-sql-queries"></a>Zusammengesetzte SQL-Abfragen

Sie können die oben angegebenen Operatoren zu komplexeren Abfragen zusammensetzen. Da Cosmos DB geschachtelte Container unterstützt, können Sie die Zusammensetzung entweder verketten oder schachteln.

### <a name="concatenation"></a>Verkettung

Die Syntax ist `input(.|.SelectMany())(.Select()|.Where())*`. Eine verkettete Abfrage kann mit einer optionalen `SelectMany`-Abfrage beginnen, gefolgt von mehreren `Select`- oder `Where`-Operatoren.

**Verkettung, Beispiel 1:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Select(family => family.parents[0])
          .Where(parent => parent.familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Verkettung, Beispiel 2:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Verkettung, Beispiel 3:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Verkettung, Beispiel 4:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

### <a name="nesting"></a>Verschachtelung

Die Syntax ist `input.SelectMany(x=>x.Q())`, wobei `Q` ein `Select`-, `SelectMany`- oder `Where`-Operator ist.

Bei einer geschachtelten Abfrage wird die innere Abfrage auf jedes Element des äußeren Containers angewendet. Ein wichtiges Feature ist, dass die innere Abfrage wie bei einer Selbstverknüpfung auf die Felder der Elemente im äußeren Container verweisen kann.

**Verschachtelung, Beispiel 1:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**Verschachtelung, Beispiel 2:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**Verschachtelung, Beispiel 3:**

- **LINQ Lambda-Ausdruck**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```


## <a name="next-steps"></a>Nächste Schritte

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Modelldokumentdaten](modeling-data.md)
