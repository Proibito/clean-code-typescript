# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)

Concetti di Clean Code adattati per TypeScript.  
Ispirato da [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript).

## Indice

  1. [Introduzione](#introduzione)
  2. [Variabili](#variabili)
  3. [Funzioni](#funzioni)
  4. [Oggetti e strutture di dati](#oggetti-e-dati-strutture)
  5. [Classi](#classi)
  6. [SOLID](#solidi)
  7. [Testing](#testing)
  8. [Concurrency](#concurrency)
  9. [Gestione degli errori](#error-handling)
  10. [Formattazione](#formattazione)
  11. [Commenti](#commenti)
  12. [Traduzioni](#traduzioni)

## Introduzione

![Immagine umoristica della stima della qualità del software come conteggio di quante imprecazioni
gridi quando leggi il codice](https://www.osnews.com/images/comics/wtfm.jpg)

Principi di ingegneria del software, dal libro di Robert C. Martin
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adattati per TypeScript. Questa non è una guida di stile. È una guida per produrre codice leggibile, riusabile, e rifattorizzabile di
[leggibile, riusabile, e rifattorizzabile](https://github.com/ryanmcdermott/3rs-of-software-architecture) software in TypeScript.

Non tutti i principi qui contenuti devono essere seguiti rigorosamente, e ancora meno saranno
universalmente condivisi. Queste sono linee guida e niente di più, ma sono
codificate in molti anni di esperienza collettiva dagli autori di
*Clean Code*.

Il nostro mestiere di ingegnere del software ha poco più di 50 anni, e stiamo
ancora imparando molto. Quando l'architettura del software sarà vecchia come l'architettura
stessa, forse allora avremo regole più stringenti da seguire. Per ora, lasciamo che queste
linee guida servano come pietra di paragone per valutare la qualità del
TypeScript che tu e il tuo team producete.

Un'altra cosa: conoscere queste linee guida non vi renderà immediatamente uno sviluppatore di software migliore, e lavorarci per molti anni non significa che non farete errori. Ogni pezzo di codice inizia come una prima bozza, come argilla bagnata che viene
plasmata nella sua forma finale. Alla fine, scalpelliamo via le imperfezioni quando lo rivediamo con i nostri colleghi. Non rimproveratevi per le prime bozze che hanno bisogno di miglioramento. Dominate il codice invece!

**[⬆ Torna in cima](#table-of-contents)**

## Variabili

### Usa nomi significativi per le variabili

Distinguere i nomi in modo tale che il lettore sappia quali sono le differenze.

**Cattivo:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Buono:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ Torna in cima](#table-of-contents)**

### Usa nomi di variabili pronunciabili

Se non riesci a pronunciarlo, non puoi discuterlo senza sembrare un idiota.

**Cattivo:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Buono:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ Torna in cima](#table-of-contents)**

### Usa lo stesso vocabolario per lo stesso tipo di variabile

**Bad:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Good:**

```ts
function getUser(): User;
```

**[⬆ Torna in cima](#table-of-contents)**

### Usa nomi ricercabili

Leggeremo più codice di quanto ne scriveremo mai. È importante che il codice che scriviamo sia leggibile e ricercabile. Non nominando *non* (not in inglese) le variabili che finiscono per essere significative per la comprensione del nostro programma, confonderanno i nostri lettori. Rendete i vostri nomi ricercabili. Strumenti come [ESlint](https://eslint.org/docs/rules/no-magic-numbers) possono aiutare ad identificare costanti senza nome.

**Cattivo:**

```ts
// Cosa cavolo vuol dire 86400000?
setTimeout(restart, 86400000);
```

**Buono:**

```ts
// Dichiara le costanti tutte maiuscole
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ Torna in cima](#table-of-contents)**

### Usa le variabili esplicative

**Bad:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // itera nella Map utenti
}
```

**Good:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // itera nella Map utenti
}
```

**[⬆ Torna in cima](#table-of-contents)**

### Evitare la mappatura mentale

L'esplicito è meglio dell'implicito.  
*La chiarezza è fondamentale.*

**Bad:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Good:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ Torna in cima](#table-of-contents)**

### Non aggiungere un contesto non necessario

Se il nome della tua classe/tipo/nome ha già un significato proprio, non ripetetelo nel nome di variabile.

**Cattivo:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Buono:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```
**[⬆ Torna in cima](#table-of-contents)**

### Usa argomenti predefiniti invece che esplicitarli nel codice

Gli argomenti predefiniti sono spesso più puliti che specificarli nella logica della funzione.

**Cattivo:**

``ts
funzione loadPages(count?: numero) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Bene:**

``ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ Torna in cima](#table-of-contents)**

### Usare enum per documentare l'intento

Gli enum possono aiutarvi a documentare l'intento del codice. Per esempio quando ci preoccupiamo che i valori siano
diversi piuttosto che il loro valore esatto.

**Cattivo:**

```ts
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary',
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**Buono:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**[⬆ Torna in cima](#table-of-contents)**

## Funzioni

### Argomenti delle funzioni (idealmente 2 o meno)

Limitare il numero di parametri della funzione è incredibilmente importante perché rende più facile testare la tua funzione.
Avere più di tre parametri porta ad un'esplosione combinata in cui si devono testare tonnellate di casi diversi con ogni argomento separato.  

Uno o due argomenti è il caso ideale, e tre dovrebbero essere evitati se possibile. Tutte le funzioni con più di 2 argomenti devono essere scritte solo dopo una decisione molto ponderata.
Di solito, se avete più di due argomenti, la vostra funzione sta cercando di fare troppo.
Nei casi in cui non è così, la maggior parte delle volte un oggetto di livello superiore sarà sufficiente come argomento.  

Considerate l'uso di letterali di oggetto se vi trovate ad avere bisogno di molti argomenti.  

Per rendere ovvio quali proprietà la funzione si aspetta, potete usare la sintassi [destrutturazione](https://basarat.gitbook.io/typescript/future-javascript/destructuring).
Questo ha alcuni vantaggi:

1. Quando qualcuno guarda la firma della funzione, deve essere immediatamente chiaro quali proprietà vengono usate.

2. Può essere usata per simulare parametri nominati.

3. La destrutturazione clona anche i valori primitivi specificati dell'oggetto argomento passato nella funzione. Questo può aiutare a prevenire effetti collaterali. Nota: gli oggetti e gli array che vengono destrutturati dall'oggetto argomento NON vengono clonati.

4. TypeScript vi avverte delle proprietà inutilizzate, che sarebbero impossibili senza la destrutturazione.

**Cattivo:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Buono:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

Si può migliorare ulteriormente la leggibilità usando [type alias](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ Torna in cima](#table-of-contents)**

### Le funzioni dovrebbero fare una cosa sola

Questa è di gran lunga la regola più importante nell'ingegneria del software. Quando le funzioni fanno più di una cosa, sono più difficili da comporre, testare e ragionare. Quando si può isolare una funzione per una sola azione, essa può essere rifattorizzata facilmente e il codice risulterà molto più pulito. Se imparerai da questa guida, solo a questo consiglio, sarai già più avanti di molti sviluppatori.

**Bad:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ Torna in cima](#table-of-contents)**

### I nomi delle funzioni dovrebbero dire cosa fanno

**Bad:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// È difficile dire da dove arriva il nome della funzione e cosa è aggiunto
addToDate(date, 1);
```

**Good:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ Torna in cima](#table-of-contents)**

### Le funzioni dovrebbero essere solo un livello di astrazione

Quando avete più di un livello di astrazione la vostra funzione di solito sta facendo troppo. Dividere le funzioni porta alla riusabilità e a test più facili.

**Bad:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ Torna in cima](#table-of-contents)**

### Rimuovere il codice duplicato

Fate del vostro meglio per evitare il codice duplicato.
Il codice duplicato è un male perché significa che c'è più di un posto per modificare qualcosa se avete bisogno di cambiare qualche logica.  

Immaginate di gestire un ristorante e di tenere traccia del vostro inventario: tutti i vostri pomodori, cipolle, aglio, spezie, ecc.
Se avete più liste su cui tenete questo, allora tutte devono essere aggiornate quando servite un piatto con pomodori.
Se hai solo una lista, c'è solo un posto da aggiornare!  

Spesso si ha codice duplicato perché si hanno due o più cose leggermente diverse, che hanno molto in comune, ma le loro differenze costringono ad avere due o più funzioni separate che fanno molte delle stesse cose. Rimuovere il codice duplicato significa creare un'astrazione che può gestire questo insieme di cose diverse con una sola funzione/modulo/classe.  

Ottenere la giusta astrazione è fondamentale, ecco perché dovreste seguire i principi [SOLID](#solid). Cattive astrazioni possono essere peggio del codice duplicato, quindi fate attenzione! Detto questo, se potete fare una buona astrazione, fatelo! Non ripetetevi, altrimenti vi troverete ad aggiornare più posti ogni volta che vorrete cambiare una cosa.

**Cattivo:**

``ts
funzione showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      esperienza,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      esperienza,
      portafoglio
    };

    render(data);
  });
}
```

**Bene:**

``ts
classe Sviluppatore {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

classe Manager {
  // ...
  getExtraDetails() {
    return {
      portafoglio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    dati costanti = {
      expectedSalary,
      esperienza,
      extra,
    };

    render(dati);
  });
}
```

Dovresti essere critico riguardo alla duplicazione del codice. A volte c'è un compromesso tra codice duplicato e aumento della complessità introducendo un'astrazione non necessaria. Quando due implementazioni di due moduli diversi sembrano simili ma vivono in domini diversi, la duplicazione potrebbe essere accettabile e preferita all'estrazione del codice comune. Il codice comune estratto, in questo caso, introduce una dipendenza indiretta tra i due moduli.

**[⬆ back to top](#table-of-contents)**