# EXISTS

```EXISTS``` wordt in SQL gebruikt om te controleren of er ten minste één rij bestaat die voldoet aan een bepaalde voorwaarde. Het is een booleaanse operator: het resultaat is altijd ```TRUE``` of ```FALSE```.

```EXISTS``` wordt toegepast in een ```WHERE```-clause en werkt met een gecorreleerde subquery. De subquery wordt per rij van de buitenste query geëvalueerd en bepaalt of die rij wordt opgenomen in het resultaat.

## Hoe EXISTS werkt

```sql
SELECT *
FROM suppliers s
WHERE EXISTS (
    SELECT 1
    FROM sales sa
    WHERE sa.supplier_id = s.supplier_id
      AND sa.month = 1
);
```

Belangrijke eigenschappen:

- de subquery hoeft geen specifieke kolommen te selecteren  
- ```SELECT``` 1 is conventioneel en semantisch neutraal  
- SQL stopt met evalueren zodra de eerste match is gevonden  
- de inhoud van de rij is irrelevant.

Bovenstaande query selecteert alle leveranciers waarvoor ten minste één verkoop in maand 1 bestaat. ```EXISTS``` controleert uitsluitend of zo’n verkoop voorkomt en haalt geen data uit de sales-tabel zelf op.

## EXISTS vs JOIN

Hoewel ```EXISTS``` en ```JOIN``` soms hetzelfde resultaat opleveren, drukken ze verschillende intenties uit.

- Een ```JOIN``` wordt gebruikt wanneer je data uit meerdere tabellen wilt combineren.  
- ```EXISTS``` wordt gebruikt wanneer je alleen wilt filteren op het bestaan van gerelateerde rijen.

Met een ```JOIN``` introduceer je extra rijen en kolommen in je resultaatset. Met ```EXISTS``` blijft de vorm van het resultaat onveranderd.

## EXISTS VS IN

```EXISTS``` wordt vaak verward met de ```IN```-operator, maar ze dienen een ander doel. ```IN``` controleert of een waarde voorkomt in een expliciete set van waarden. Dit is in feite een directe vergelijking van een kolom met een expliciete, of afgeleide, lijst. 

```sql
WHERE supplier_id IN (
    SELECT supplier_id
    FROM sales
    WHERE month = 1
)
```

Voor een klein aantal waarden is ```IN``` doorgaans zeer efficiënt en eenvoudig. Naarmate de set groter wordt, kan ```IN``` echter minder schaalbaar zijn, terwijl ```EXISTS``` juist goed werkt bij grotere of gecorreleerde subqueries, omdat het alleen hoeft vast te stellen of er minimaal één match bestaat.

## NOT EXISTS

```EXISTS``` wordt vaak gecombineerd met ```NOT``` om afwezigheid expliciet te maken. Dit is nuttig voor:

- Ontbrekende relaties  
- incomplete data  
- uitzonderingen in raportages

## Conclusie

```EXISTS``` is een relationele operator die expliciet maakt wat vaak impliciet blijft bij joins en filters:

- het drukt relatie-existentie uit;  
- het houdt je resultaatset intact;

Door ```EXISTS``` te gebruiken maak je in je SQL duidelijk dat je niet geïnteresseerd bent in data, maar in het feit dát er data is.

## sources:

[IN vs EXISTS in SQL - GeeksforGeeks](https://www.geeksforgeeks.org/sql/in-vs-exists-in-sql/)

[SQL EXISTS Operator - W3Schools](https://www.w3schools.com/sql/sql_exists.asp)

[SQL | EXISTS - GeeksforGeeks](https://www.geeksforgeeks.org/sql/sql-exists/)