# Facades  

Facades abstrahieren den Zugriff auf die Datenbank. Dafür bieten diese Schnittstellen für das Laden und das Manipulieren von Daten. Jede Datenbanktabelle bestitzt eine eigene Facade, die den Zugriff abstrahiert.


## Composite-Facades
Composite Facades vereinen mehrere Facades zu einer einzigen. Diese Facades werden normalerweise mit SQL zusammengejoint. Die Composite Facades besitzen alle Filter und Order-Bys der Facades, die sie kapseln. Diese Filter und Order-Bys können direkt über den Programm Code angesprochen werden. 
Die verschiedenen Filter und Order-Bys werden bei den Select-Operationen automatisch mit *AND* zusammengefügt. Diese Verhalten kann folgendermaßen verändert werden.

```typescript
const compositeFacade = new CompositeFacade(tableAlias?: string);

/* 
  Ändert den Operator, der zum Zusammenführen 
  der Filter verwendet wird.
*/
compositeFacade.sqlOperator(value: SQLOperator);

/* 
  Wenn dieser Wert auf false gesetzt wird, dann
  werden die Filter nicht automatisch kombiniert.
  Daher muss man sich selbst darum kümmern, dass
  die Filter kombiniert werden. 
*/
compositeFacade.autoCombineFilter(value: boolean);
```
### Composite-Facades Joins

Standardmäßig werden alle Tabllen, die in der Facade definiert wurden, gejoint. Jeder Join kann einzeln deaktiviert werden. Dazu exisitieren in den Composite-Facades Set-Methoden.

```typescript
Beispiel:

set withIngredientsJoin(value: boolean) {  
    this._withIngredientsJoin = value;  
}
```
___
## Datenbank Select-Operationen   

### Entität eines Models mit einer ID laden  
  
Wenn die Entität gefunden mit der ID gefunden wurde,   
dann wird diese retouniert, ansonsten wird *undefined* zurückgegeben.  
  
```typescript
const facade = new Facade(tableAlias?: string);
const entity = await facade.getById(id: number);  
```  
  
### Alle Entitäten eines Models laden  
  
Wenn Entitäten gefunden wurde, dann werden diese retouniert,   
ansonsten wird ein leeres Array zurückgegeben.  
  
```typescript  
const facade = new Facade(tableAlias?: string);
const entities = await facade.get();  
```  
  
### Erste Entität eines Models laden  
  
Es wird die erste Entität retouniert, die gefunden wurde. Wenn mehr als eine Enität zurückgegeben wurde, dann wird ein Fehler geworfen. Wenn keine Entität zurückgegeben wurde, dann wird *undefined* retouniert.  
  
```typescript  
const facade = new Facade(tableAlias?: string);
const entity = await facade.getOne();  
```  
___  
  
## SQL-Where Filter
  
Jede Facade besitzt einen eigenen Filter. Dieser kann benutzt werden, um eine Select-Query zu filtern. Dazu muss eine Filterbedingung mit *addFilterCondition* hinzugefügt werden. Diese behinhaltet den Namen des Attributes und den dazugehörenen Wert. Danach wird der Vergleichsoperator (=, <, >, ...) angegeben. Als optionaler Parameter kann noch ein SQL-Operator übergeben werden. Dieser Operator kann entweder *AND* oder *OR* sein. Dies sollte aber nur verwendet werden, wenn danach noch ein Filterattribut angegeben wird, da der SQL-Operator die beiden Klauseln verbindet.  
  
```typescript  
const facade = new Facade(tableAlias?: string);  
const filter: Filter = facade.filter;  
  
filter.addFilterCondition(name: string, value: any, sqlOperator: SQLComparisonOperator, operator?: SQLOperator);  
```  
  
Optional kann der Operator, der zwei Klauseln verbindet, auch nach dem *addFilterCondition* angegeben werden.  
  
```typescript 
filter.addOperator(operator: SQLOperator);  
```  
  
Zusätzlich kann noch ein Sub-Filter zum aktuellen Filter hinzugefügt werden. Dieser Sub-Filter wird als ein Ausdruck und geklammert in den Filter eingefügt.  
z.b.: *... AND ( attr = value AND ... ) ...*  
  
```javascript  
const anotherFilter = new Filter(tableAlias: string);  
filter.addSubFilter(anotherFilter);  
```  
  
Es können beliebig viele Filterklauseln angegeben werden. Der Filter muss aber vor der Ausführung der Datenbankoperationen definiert werden.  
___  
  
Alle Filterklauseln können aus dem Filter über die Facade oder über den Filter entfernt werden.  
  
```javascript  
const facade = new Facade(tableAlias?: string);  
facade.clearFilter();  
  
const filter: Filter = new Filter();  
filter.clear();  
```  
  
___  
  
## SQL-Order By

Das Ergebnis einer Abfrage kann mit einem Order-By sortiert werden. Dazu muss auf der Facade direkt eine Ordnung auf einem Attribut angegben werden. Die Ordnung kann entweder aufsteigend (ASC) oder absteigend (DESC) sein.

```typescript  
const facade = new Facade(tableAlias?: string);  
facade.addOrderBy(attribute: string, order: SQLOrder);  
```  

Es können beliebig viele Order-Bys definiert werden.

___

## Post-Process Filter

Den Facades kann zusätzlich noch eine Methode übergeben werden, die nach den verschiedenen Select-Operationen ausgeführt wird. Diese Methode kann die Ergebnisse noch zusätzlich filtern, was eventuell mit SQL nicht möglich war. Die Funktion wird automatisch aufgerufen, daher muss sie nur definiert werden.

Die Signatur der Methode sieht folgendermaßen aus.
```typescript
((entities: EntityType[]) => EntityType[]) 
```

Beispielhafte Definition der Prozess-Methode für die ImageFacade. Nach der Definition muss nichts mehr gemacht werden.
```typescript
const facade = new ImageFacade();
facade.postProcessFilter = ((entities: Image[]) => {  
   // process entities
   return entities;  
});
```
___