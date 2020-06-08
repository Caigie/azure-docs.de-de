---
title: 'Date_Bucket (Transact-SQL): Azure SQL Edge (Vorschau)'
description: Informationen zur Verwendung von Date_Bucket in Azure SQL Edge (Vorschau)
keywords: Date_Bucket, SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 7cedc19c1b2a19fb26677a9426300d19c5396bd4
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682729"
---
# <a name="date_bucket-transact-sql"></a>Date_Bucket (Transact-SQL)

Diese Funktion gibt den datetime-Wert zurück, der dem Anfang jedes datetime-Buckets entspricht, ab dem Standardursprungswert `1900-01-01 00:00:00.000`.

Eine Übersicht über alle Datums- und Uhrzeitdatentypen und zugehörigen Funktionen (Transact-SQL) finden Sie unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql/).

[Transact-SQL-Syntaxkonventionen](/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql/)

`DATE_BUCKET` verwendet einen Standardwert von `1900-01-01 00:00:00.000` für das Ursprungsdatum, d. h. 12:00 Uhr am Montag, dem 1. Januar 1900.

## <a name="syntax"></a>Syntax

```sql
DATE_BUCKET (datePart, number, date)
```

## <a name="arguments"></a>Argumente

*datePart*

Der Teil von *date*, der mit dem Parameter „number“ verwendet wird. Ex. Jahr, Monat, Minute, Sekunde usw.

> [!NOTE]
> `DATE_BUCKET` akzeptiert keine benutzerdefinierten Variablenentsprechungen für die *datePart*-Argumente.
  
|*datePart*|Abkürzungen|  
|---|---|
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  

*Zahl*

Der Integerwert, der die Breite des Buckets in Kombination mit dem *datePart*-Argument bestimmt. Dies stellt die Breite des datePart-Buckets ab der Ursprungszeit dar. **`This argument cannot be a negative integer value`** . 

*date*

Ein Ausdruck, der in einen der folgenden Werte aufgelöst werden kann:

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2**
+ **smalldatetime**
+ **time**

Für *date* akzeptiert `DATE_BUCKET` einen Spaltenausdruck, einen Ausdruck oder eine benutzerdefinierte Variable, wenn diese in einen der oben genannten Datentypen aufgelöst werden.

## <a name="return-type"></a>Rückgabetyp

Der Datentyp des Rückgabewerts für diese Methode ist dynamisch. Der Rückgabetyp hängt von dem Argument ab, das für `date` angegeben wird. Wenn für `date` ein gültiger Eingabedatentyp angegeben wird, gibt `DATE_BUCKET` denselben Datentyp zurück. `DATE_BUCKET` löst einen Fehler aus, wenn für den `date`-Parameter ein Zeichenfolgenliteral angegeben wird.

## <a name="return-values"></a>Rückgabewerte

### <a name="understanding-the-output-from-date_bucket"></a>Grundlegendes zur Ausgabe von `DATE_BUCKET`

`Date_Bucket` gibt den aktuellsten Datums- oder Uhrzeitwert zurück, der dem datePart- und dem number-Parameter entspricht. In den folgenden Ausdrücken gibt `Date_Bucket` z. B. den Ausgabewert `2020-04-13 00:00:00.0000000` zurück, da die Ausgabe basierend auf einwöchigen Buckets aus der Standardursprungszeit von `1900-01-01 00:00:00.000` berechnet wird. Der Wert `2020-04-13 00:00:00.0000000` ist 6.276 Wochen vom Ursprungswert `1900-01-01 00:00:00.000` entfernt. 

```sql
declare @date datetime2 = '2020-04-15 21:22:11'
Select DATE_BUCKET(wk, 1, @date)
```

Für alle folgenden Ausdrücke wird der gleiche Ausgabewert von `2020-04-13 00:00:00.0000000` zurückgegeben. Dies liegt daran, dass `2020-04-13 00:00:00.0000000` 6.276 Wochen vom Ursprungsdatum entfernt und 6.276 durch 2, 3, 4 und 6 teilbar ist.

```sql
declare @date datetime2 = '2020-04-15 21:22:11'
Select DATE_BUCKET(wk, 2, @date)
Select DATE_BUCKET(wk, 3, @date)
Select DATE_BUCKET(wk, 4, @date)
Select DATE_BUCKET(wk, 6, @date)
```

Die Ausgabe für den unten angegebenen Ausdruck, der 6.275 Wochen von der Ursprungszeit entfernt ist.

```sql
declare @date datetime2 = '2020-04-15 21:22:11'
Select DATE_BUCKET(wk, 5, @date)
```

## <a name="datepart-argument"></a>datepart-Argument

**dayofyear**, **day** und **weekday** geben den gleichen Wert zurück. Jedes *datepart*-Argument und die jeweils zugehörigen Abkürzungen geben den gleichen Wert zurück.
  
## <a name="number-argument"></a>number-Argument

Das *number*-Argument kann den Bereich von positiven **int**-Werten nicht überschreiten. In den folgenden Anweisungen überschreitet das Argument für *number* den Bereich von **int** um 1. Die folgende Anweisung gibt folgende Fehlermeldung zurück: „`Msg 8115, Level 16, State 2, Line 2. Arithmetic overflow error converting expression to data type int."`
  
```sql
declare @date datetime2 = '2020-04-30 00:00:00'
Select DATE_BUCKET(dd, 2147483648, @date)
```  

Wenn ein negativer Wert für „number“ an die `Date_Bucket`-Funktion übergeben wird, wird der folgende Fehler zurückgegeben. 

```sql
Msg 9834, Level 16, State 1, Line 1
Invalid bucket width value passed to date_bucket function. Only positive values are allowed.
````

## <a name="date-argument"></a>date-Argument  

`DATE_BUCKET` gibt den Basiswert zurück, der dem Datentyp des `date`-Arguments entspricht. Im folgenden Beispiel wird ein Ausgabewert mit dem datetime2-Datentyp zurückgegeben. 

```sql
Select DATE_BUCKET(dd, 10, SYSUTCDATETIME())
```

## <a name="remarks"></a>Bemerkungen

Verwenden Sie `DATE_BUCKET` in den folgenden Klauseln:

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE

## <a name="examples"></a>Beispiele

### <a name="a-calculating-date_bucket-with-a-bucket-width-of-1-from-the-origin-time"></a>A. Berechnen von Date_Bucket mit einer Bucketbreite von 1 ab der Ursprungszeit

Jede dieser Anweisungen erhöht *date_bucket* mit einer Bucketbreite von 1 ab der Ursprungszeit:

```sql
declare @date datetime2 = '2020-04-30 21:21:21'
Select 'Week',  DATE_BUCKET(wk, 1, @date)
Union All
Select 'Day',  DATE_BUCKET(dd, 1, @date)
Union All
Select 'Hour',  DATE_BUCKET(hh, 1, @date)
Union All
Select 'Minutes',  DATE_BUCKET(mi, 1, @date)
Union All
Select 'Seconds',  DATE_BUCKET(ss, 1, @date)
```

Hier ist das Resultset.

```sql
Week    2020-04-27 00:00:00.0000000
Day     2020-04-30 00:00:00.0000000
Hour    2020-04-30 21:00:00.0000000
Minutes 2020-04-30 21:21:00.0000000
Seconds 2020-04-30 21:21:21.0000000
```

### <a name="b-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>B. Verwenden von Ausdrücken als Argumente für den number-Parameter und den date-Parameter

In diesen Beispielen werden verschiedene Typen von Ausdrücken als Argumente für die Parameter *number* und *date* verwendet. Diese Beispiele werden mithilfe der Datenbank „AdventureWorksDW2017“ erstellt.
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Angeben von benutzerdefinierten Variablen als Argumente für number und date  

In diesem Beispiel werden benutzerdefinierte Variablen als Argumente für *number* und *date* angegeben:
  
```sql
DECLARE @days int = 365,
        @datetime datetime2 = '2000-01-01 01:01:01.1110000'; /* 2000 was a leap year */;  
SELECT Date_Bucket(day, @days, @datetime);
```

Hier ist das Resultset.

```sql
---------------------------
1999-12-08 00:00:00.0000000

(1 row affected)
```  

#### <a name="specifying-a-column-as-date"></a>Angeben einer Spalte als date-Parameter

Im folgenden Beispiel berechnen wir die Summe von OrderQuantity und die Summe von UnitPrice, gruppiert über wöchentliche date-Buckets.
  
```sql
SELECT
    Date_Bucket(week, 1 ,cast(Shipdate as datetime2)) AS ShippedDateBucket
    ,Sum(OrderQuantity)  As SumOrderQuantity
    ,Sum(UnitPrice) As SumUnitPrice
FROM dbo.FactInternetSales FIS
where Shipdate between '2011-01-03 00:00:00.000' and '2011-02-28 00:00:00.000'
Group by Date_Bucket(week, 1 ,cast(Shipdate as datetime2))
order by 1
```  

Hier ist das Resultset.
  
```sql
ShippedDateBucket           SumOrderQuantity SumUnitPrice
--------------------------- ---------------- ---------------------
2011-01-03 00:00:00.0000000 21               65589.7546
2011-01-10 00:00:00.0000000 27               89938.5464
2011-01-17 00:00:00.0000000 31               104404.9064
2011-01-24 00:00:00.0000000 36               118525.6846
2011-01-31 00:00:00.0000000 39               123555.431
2011-02-07 00:00:00.0000000 35               109342.351
2011-02-14 00:00:00.0000000 32               107804.8964
2011-02-21 00:00:00.0000000 37               119456.3428
2011-02-28 00:00:00.0000000 9                28968.6982
```  

#### <a name="specifying-scalar-system-function-as-date"></a>Angeben einer skalaren Systemfunktion als Argument für date

In diesem Beispiel wird `SYSDATETIME` für *date* angegeben. Welcher Wert genau zurückgegeben wird, hängt davon ab, an welchem Tag und zu welcher Uhrzeit die Anweisung ausgeführt wird:
  
```sql
SELECT Date_Bucket(wk, 10, SYSDATETIME());  
```  

Hier ist das Resultset.

```sql
---------------------------
2020-03-02 00:00:00.0000000

(1 row affected)
```  

#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Angeben von skalaren Unterabfragen und skalaren Funktionen als Argumente für number und date

In diesem Beispiel werden skalare Unterabfragen (`MAX(OrderDate)`) als Argumente für *number* und *date* verwendet. `(SELECT top 1 CustomerKey FROM dbo.DimCustomer where GeographyKey > 100)` dient als Beispielargument für den Parameter „number“, das veranschaulicht, wie ein *number*-Argument aus einer Werteliste ausgewählt wird.
  
```sql
SELECT DATE_BUCKET(week,(SELECT top 1 CustomerKey FROM dbo.DimCustomer where GeographyKey > 100),  
    (SELECT MAX(OrderDate) FROM dbo.FactInternetSales));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Angeben von numerischen Ausdrücken und skalaren Systemfunktionen als Argumente für number und date

In diesem Beispiel werden ein numerischer Ausdruck ((10/2)) und Skalarsystemfunktionen (SYSDATETIME) als Argumente für number und date verwendet.
  
```sql
SELECT Date_Bucket(week,(10/2), SYSDATETIME());
```

#### <a name="specifying-an-aggregate-window-function-as-number"></a>Angeben einer Aggregatfensterfunktion als Argument für number

In diesem Beispiel wird eine Aggregatfensterfunktion als Argument für *number* verwendet.
  
```sql
Select 
    DISTINCT DATE_BUCKET(day, 30, Cast([shipdate] as datetime2)) as DateBucket,
    First_Value([SalesOrderNumber]) OVER (Order by DATE_BUCKET(day, 30, Cast([shipdate] as datetime2))) as First_Value_In_Bucket,
    Last_Value([SalesOrderNumber]) OVER (Order by DATE_BUCKET(day, 30, Cast([shipdate] as datetime2))) as Last_Value_In_Bucket
    from [dbo].[FactInternetSales]
Where ShipDate between '2011-01-03 00:00:00.000' and '2011-02-28 00:00:00.000'
order by DateBucket
GO  
``` 

## <a name="see-also"></a>Weitere Informationen

[CAST und CONVERT &#40;Transact-SQL&#41;](/sql/t-sql/functions/cast-and-convert-transact-sql/)
