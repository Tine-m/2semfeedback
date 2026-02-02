# Introduktion til Lambda-udtryk og Streams i Java

Dette er en kort introduktion til **lambda-udtryk** og **streams**, der forhåbentlig kan give en intuitiv forståelse af emnet – ikke at dække alle detaljer.

---

## Hvad er et lambda-udtryk?

Et **lambda-udtryk** er en **lille funktion uden navn**.

Det bruges, når vi vil beskrive **hvad der skal ske med ét element ad gangen**.

### Eksempel

```java
task -> System.out.println(task.getTitle())
```

### Læs det sådan
> “For en task, print taskens titel.”

---

## Hvorfor bruge lambda-udtryk?

Før Java 8 brugte man ofte `for`-loops:

```java
for (Task task : tasks) {
    System.out.println(task.getTitle());
}
```

Med lambda-udtryk kan det skrives kortere og mere læsbart:

```java
tasks.forEach(task -> System.out.println(task.getTitle()));
```

### Fordele
- Mindre kode
- Mere læsbart
- Fokus på **hvad** der sker – ikke **hvordan**

---

## Lambda-syntaks (det vigtigste)

```java
parameter -> handling
```

### Eksempler

```java
x -> System.out.println(x)
task -> task.getTitle()
task -> task.getDueDate().isBefore(LocalDate.now())
```

---

## Hvad er en stream?

En **stream** er en måde at behandle elementer i en collection **trin for trin**.

Tænk på det som en pipeline:

```
Liste → filter → sort → resultat
```

Vigtige pointer:
- En stream ændrer **ikke** den oprindelige liste
- En stream bruges til at **udvælge eller bearbejde data**

---

## Stream-eksempel: Forfaldne (overdue) tasks

```java
tasks.stream()
     .filter(task -> task.getDueDate().isBefore(LocalDate.now()))
     .toList();
```

### Læs det som en sætning
> Tag alle tasks  
> → behold kun dem der er forfaldne  
> → returnér dem som en liste

---

## Lambda og stream hænger sammen

- **Stream**: går igennem alle elementer
- **Lambda**: fortæller hvad der skal ske med hvert element

### Eksempel uden filter

```java
tasks.forEach(task ->
        System.out.println(task.getTitle())
);
```

### Betyder
> For hver task, print titlen

---

## De vigtigste 3 stream-metoder

### `forEach`
Udfører noget for hvert element.

```java
tasks.forEach(task -> System.out.println(task));
```

---

### `filter`
Beholder kun elementer, der opfylder en betingelse.

```java
.filter(task -> task.getTitle().contains("garden"))
```

---

### `sorted`
Sorterer elementer.

```java
.sorted(Comparator.comparing(Task::getDueDate))
```

---

### **collect / toList**:  hvad resultatet skal ende som

collect **afslutter en stream og samler resultatet op**.

```java
List<Task> overdueTasks =
    tasks.stream()
         .filter(task -> task.getDueDate().isBefore(LocalDate.now()))
         .collect(Collectors.toList());```

Læs det sådan:

Gå igennem tasks
→ udvælg de forfaldne
→ saml dem i en liste

### `collect` vs `toList()`

I nyere Java-versioner kan man ofte skrive:

```java
.toList();
```
i stedet for 

```java
.collect(Collectors.toList());
```
#### Forklaring

- `collect(...)` er den **generelle måde** at afslutte en stream og samle resultatet
- `toList()` er en **kortere og mere læsbar genvej** til den mest almindelige brug

Begge løsninger er korrekte og gør det samme i denne sammenhæng.

> **Tommelfingerregel:**  
> Brug `toList()` når du bare vil have en `List`, og `collect(...)` når du vil samle data på en mere avanceret måde.


---

## Huskeregel

> **Lambda = hvad der skal ske**  
-  `filter`  en **betingelse** (som en `if`)
-  `forEach` en **handling**
-  `sorted` en **sammenligning**


> **Stream = hvordan vi går igennem data, f.eks**
  - `forEach` gør noget for hvert element
  - `filter` udvælg elementer
  - `sorted` sortér elementer

> **collect = hvad skal resultatet ende som?**
Uden `collect` (eller `toList`) får du intet resultat.
