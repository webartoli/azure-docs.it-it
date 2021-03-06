---
title: Binding di Azure Cosmos DB in Funzioni di Azure | Documentazione Microsoft
description: Informazioni su come usare i binding di Azure Cosmos DB in Funzioni di Azure.
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: Funzioni di Azure, Funzioni, elaborazione eventi, calcolo dinamico, architettura senza server
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: fb79e2ad7514ae2cf48b9a5bd486e54b9b407bee
ms.contentlocale: it-it
ms.lasthandoff: 08/29/2017

---
# <a name="azure-functions-cosmos-db-bindings"></a>Binding di Azure Cosmos DB in Funzioni di Azure
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Questo articolo illustra come configurare e scrivere il codice di binding di Azure Cosmos DB in Funzioni di Azure. Funzioni di Azure supporta i binding di input e output per Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Per altre informazioni su Cosmos DB, vedere [Introduzione a Cosmos DB](../documentdb/documentdb-introduction.md) e [Compilare un'applicazione console Cosmos DB](../documentdb/documentdb-get-started.md).

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>Binding di input dell'API DocumentDB
Il binding di input di DocumentDB recupera un documento di DocumentDB e lo passa al parametro di input denominato della funzione. L'ID documento può essere determinato in base al trigger che richiama la funzione. 

Il binding di input di DocumentDB presenta le seguenti proprietà *function.json*:

|Proprietà  |Descrizione  |
|---------|---------|
|**nome**     | Nome del parametro di binding che rappresenta il documento nella funzione.  |
|**type**     | Il valore deve essere impostato su `documentdb`.        |
|**databaseName** | Database che contiene il documento.        |
|**collectionName**  | Nome della raccolta che contiene il documento. |
|**id**     | ID del documento da recuperare. Questa proprietà supporta i parametri di binding. Per altre informazioni, vedere [Associare le proprietà di input personalizzate in un'espressione di associazione](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). |
|**sqlQuery**     | Query SQL di Cosmos DB usata per recuperare più documenti. La query supporta i binding di runtime, come nell'esempio: `SELECT * FROM c where c.departmentId = {departmentId}`.        |
|**connessione**     |Nome dell'impostazione app contenente la stringa di connessione di Cosmos DB.        |
|**direction**     | Il valore deve essere impostato su `in`.         |

Non è possibile impostare entrambe le proprietà **id** e **sqlQuery**. Se non ne viene impostata nessuna delle due, viene recuperata l'intera raccolta.

## <a name="using-a-documentdb-api-input-binding"></a>Usare un binding di input dell'API DocumentDB

* Nelle funzioni C# e F#, quando la funzione termina correttamente, le modifiche apportate al documento di input tramite i parametri di input denominati vengono rese automaticamente persistenti. 
* Nelle funzioni di JavaScript gli aggiornamenti non vengono eseguiti automaticamente al termine della funzione. Per eseguire gli aggiornamenti usare invece `context.bindings.<documentName>In` e `context.bindings.<documentName>Out`. Vedere l'[esempio di JavaScript](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Esempio di input per il singolo documento
Si supponga di avere il seguente binding di input dell'API DocumentDB nella matrice `bindings` di function.json:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Vedere l'esempio specifico del linguaggio che usa questo binding di input per aggiornare il valore di testo del documento.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>Esempio di input in C# #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>Esempio di input in F# #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Per questo esempio è necessario un file `project.json` che specifichi le dipendenze NuGet `FSharp.Interop.Dynamic` e `Dynamitey`:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Per aggiungere un file `project.json`, vedere l'argomento relativo alla [gestione dei pacchetti F #](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>Esempio di input in JavaScript

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Esempio di input con più documenti

Si supponga che si desideri recuperare più documenti specificati da una query SQL, mediante un trigger di coda per personalizzare i parametri di query. 

In questo esempio, il trigger di coda offre un parametro `departmentId`. Un messaggio di coda `{ "departmentId" : "Finance" }` restituirà tutti i record per il reparto finanziario. Usare il codice seguente in *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}"
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Esempio di input con più documenti in C#

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>Esempio di input con più documenti in JavaScript

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a id="docdboutput"></a>Binding di output dell'API DocumentDB
Il binding di output dell'API DocumentDB consente di scrivere un nuovo documento in un database di Azure Cosmos DB. In *function.json* ha le proprietà seguenti:

|Proprietà  |Descrizione  |
|---------|---------|
|**nome**     | Nome del parametro di binding che rappresenta il documento nella funzione.  |
|**type**     | Il valore deve essere impostato su `documentdb`.        |
|**databaseName** | Database contenente la raccolta in cui viene creato il documento.     |
|**collectionName**  | Nome della raccolta in cui viene creato il documento. |
|**createIfNotExists**     | Valore booleano che indica se la raccolta viene creata quando non esiste. Il valore predefinito è *false*. Il motivo è che le nuove raccolte vengono create con una velocità effettiva riservata, che ha implicazioni in termini di costi. Per altre informazioni, visitare la [pagina dei prezzi](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**connessione**     |Nome dell'impostazione app contenente la stringa di connessione di Cosmos DB.        |
|**direction**     | Il valore deve essere impostato su `out`.         |

## <a name="using-a-documentdb-api-output-binding"></a>Usare un binding di output dell'API DocumentDB
Questa sezione illustra come usare il binding di output dell'API DocumentDB nel codice di funzione.

Per impostazione predefinita, quando si scrive nel parametro di output della funzione, viene creato un documento nel database. L'ID di questo documento è un GUID generato automaticamente. È possibile specificare l'ID del documento di output specificando la proprietà `id` nell'oggetto JSON passato al parametro di output. 

>[!Note]  
>Quando si specifica l'ID di un documento esistente, questo viene sovrascritto dal nuovo documento di output. 

Per visualizzare più documenti, è anche possibile definire l'associazione a `ICollector<T>` o `IAsyncCollector<T>` dove `T` è uno dei tipi supportati.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>Esempio di binding di output dell'API DocumentDB
Si supponga di avere il seguente binding di output dell'API DocumentDB nella matrice `bindings` di function.json:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

Ed è disponibile un binding di input di coda per una coda che riceve JSON nel formato seguente:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Si vuole creare documenti di Cosmos DB nel formato seguente per ogni record:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Vedere l'esempio specifico del linguaggio che usa questo binding di output per aggiungere i documenti al database.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Esempio di output in C# #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Esempio di output in F# #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Per questo esempio è necessario un file `project.json` che specifichi le dipendenze NuGet `FSharp.Interop.Dynamic` e `Dynamitey`:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Per aggiungere un file `project.json`, vedere l'argomento relativo alla [gestione dei pacchetti F #](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>Esempio di output in JavaScript

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```

