---
title: 'Tutorial: Trainieren und Vergleichen von Vorhersagemodellen in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In Teil zwei dieser dreiteiligen Tutorialreihe erstellen Sie zwei Vorhersagemodelle in R mit den Machine Learning Services (Vorschau) von Azure SQL-Datenbank und wählen dann das genaueste Modell aus.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ca0af9a34587f8d3a3c0502c77556975b1d8df4e
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85253833"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Tutorial: Erstellen eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

In Teil 2 dieser dreiteiligen Tutorialreihe erstellen Sie zwei Vorhersagemodelle in R und wählen dann das genaueste Modell aus. Im nächsten Teil dieser Reihe stellen Sie dieses Modell in einer Datenbank in Azure SQL-Datenbank mit Machine Learning Services für Azure SQL-Datenbank (Vorschauversion) bereit.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Trainieren von zwei Machine Learning-Modellen
> * Erstellen von Vorhersagen aus beiden Modellen
> * Vergleichen der Ergebnisse, um das genaueste Modell zu wählen

In [Teil 1](predictive-model-prepare-data-tutorial.md) haben Sie gelernt, wie Sie eine Beispieldatenbank importieren und anschließend die Daten vorbereiten, die zum Trainieren eines Vorhersagemodells in R verwendet werden sollen.

In [Teil 3](predictive-model-deploy-tutorial.md) lernen Sie, wie Sie das Modell in einer Datenbank speichern und anschließend auf der Grundlage der in den Teilen 1 und 2 entwickelten R-Skripts gespeicherte Prozeduren erstellen. Die gespeicherten Prozeduren werden in einer Datenbank ausgeführt, um Vorhersagen basierend auf neuen Daten zu treffen.

## <a name="prerequisites"></a>Voraussetzungen

* In Teil zwei dieser Reihe von Tutorials wird angenommen, dass Sie [**Teil eins**](predictive-model-prepare-data-tutorial.md) und seine Voraussetzungen abgeschlossen haben.

## <a name="train-two-models"></a>Trainieren von zwei Modellen

Um das beste Modell für die Skiverleihdaten zu finden, erstellen Sie zwei verschiedene Modelle (lineare Regression und Entscheidungsstruktur) und stellen fest, welches genauere Vorhersagen liefert. Sie werden den Datenrahmen `rentaldata` verwenden, den Sie in Teil eins dieser Reihe erstellt haben.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];

#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use rxLinMod to create a linear regression model, trained with the training data set
model_linmod <- rxLinMod(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rxDTree to create a decision tree model, trained with the training data set
model_dtree  <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Erstellen von Vorhersagen aus beiden Modellen

Verwenden Sie eine Vorhersagefunktion, um die Vermietungszahlen mithilfe jedes der trainierten Modelle vorherzusagen.

```r
#Use both models to make predictions using the test data set.
predict_linmod <- rxPredict(model_linmod, test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

predict_dtree  <- rxPredict(model_dtree,  test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_linmod);
head(predict_dtree);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Vergleichen der Ergebnisse

Jetzt möchten Sie bestimmen, welches der Modelle die besten Vorhersagen ergibt. Eine schnelle und bequeme Möglichkeit dazu bietet eine einfache Zeichenfunktion, um den Unterschied zwischen den Ist-Werten in Ihren Trainingsdaten und den vorhergesagten Daten darzustellen.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(2, 1));
plot(predict_linmod$RentalCount_Pred - predict_linmod$RentalCount, main = "Difference between actual and predicted. rxLinmod");
plot(predict_dtree$RentalCount_Pred  - predict_dtree$RentalCount,  main = "Difference between actual and predicted. rxDTree");
```

![Vergleich der beiden Modelle](./media/predictive-model-build-compare-tutorial/compare-models.png)

Offenbar ist das Entscheidungsstruktur-Modell das genauere der beiden Modelle.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „TutorialDB“ von Ihrem Server.

Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Wählen Sie im Menü auf der linken Seite im Azure-Portal **Alle Ressourcen** oder **SQL-Datenbanken** aus.
1. Geben Sie im Feld **Nach Name filtern...** die **TutorialDB** ein, und wählen Sie Ihr Abonnement aus.
1. Wählen Sie Ihre TutorialDB-Datenbank aus.
1. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Trainieren von zwei Machine Learning-Modellen
* Erstellen von Vorhersagen aus beiden Modellen
* Vergleichen der Ergebnisse, um das genaueste Modell zu wählen

Um das Machine Learning-Modell bereitzustellen, das Sie erstellt haben, fahren Sie mit Teil drei dieser Tutorialreihe fort:

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank](predictive-model-deploy-tutorial.md)
