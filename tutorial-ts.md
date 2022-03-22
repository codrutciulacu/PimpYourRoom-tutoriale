# Tutorial TypeScript

## Introducere
- TypeScript-ul este un superset sintactic al limbajului JavaScript
- El adauga cateva functionalitati pastrand ideile de baza care definesc limbajul JavaScript
- Acesta adauga un set de tipuri bine definite care poate rezolva reduce problemele aparute din cauza lipsei de tipuri

## Types
- Tipuri de date primitive:
    - **number**: stocheaza numere de orice tip
    - **string**: stocheaza un sir de caractere
    - **boolean**: stocheaza true sau false
    - **any**: este un tip generic pentru cazul in care nu stim ce vrem sa stocam
    - **Array<tip de date>**: care stocheaza un array cu elemente de un anumit tip(echivalent cu **tip[]**)
- Orice variabila trebuie sa aiba un tip anume
- Cand nu definiti un tip este este definit in mod default ca any

## Union Types
- Daca aveti o variabila al carei tip de date poate varia putem folosi un union type:
```typescript
    function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
    // OK
    printId(101);
    // OK
    printId("202");
    // Error pentru ca nu este string sau number
    printId({ myID: 22342 });
```
- Aveti grija totusi pentru ca puteti folosi proprietati/metode ale unui tip doar daca operatia este valida pentru toti membrii uniunii
- Pentru a verifica tipul de date al unei variabile puteti folosi keyword-ul **typeof**
```typescript
    function printId(id: number | string) {
        if (typeof id === "string") {
            // In this branch, id is of type 'string'
            console.log(id.toUpperCase());
        } else {
            // Here, id is of type 'number'
            console.log(id);
        }
    }
```

## Interfaces
- Pentru a defini un nou tipe de date exista doua optiuni: **type aliases** sau **interfete**
- In general aceste doua sunt echivalente, dar am observat ca majoritatea developerilor folosesc interfetele
- O iterfata se aseamana cu o clasa normala din alte limbaje, doar ca ea stocheaza doar date
- Exemplu:
```typescript
    interface Point {
        x: number;
        y: number;
    }
    
    function printCoord(pt: Point) {
        console.log("The coordinate's x value is " + pt.x);
        console.log("The coordinate's y value is " + pt.y);
    }
    
    printCoord({ x: 100, y: 100 });
```
- Pentru a extinde o interfata folosim keyword-ul **extends**: 
```typescript
    interface Animal {
        name: string
    }

    interface Bear extends Animal {
        honey: boolean
    }

    const bear = getBear() 
    bear.name
    bear.honey
```

## Type Assertions(AKA Casting)
- Pentru a ne asigura ca un element este de un anumit tip putem folosi notiunea de casting
- Pentru a face cast la un anumit tip de date folosim keyword-ul **as**:
```typescript
    const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```
- In anumite cazuri nu puteti face conversia explicita la un anumit tip asa ca folosim un mic trick si facem cast la **any** si apoi la tipul dorit:
```typescript
    const a = (expr as any) as T;
```
- Typescript-ul permite cast doar de la un tip mai specific la unul mai concret

## Literal Types
- Atunci cand variabila voastra are doar anumite valori fixe putem folosi tipurile literale:
```typescript
    let x: "hello" = "hello";
    // OK
    x = "hello";
    // ...
    x = "howdy";

    // SAU
    
    function printText(s: string, alignment: "left" | "right" | "center"){
        // ...
    }
    //OK
    printText("Hello, world", "left");
    //...
    printText("G'day, mate", "centre");

    //SAU 

    function compare(a: string, b: string): -1 | 0 | 1 {
        return a === b ? 0 : a > b ? 1 : -1;
    }

```

## Null and undefined
- In TypeScript putem, la fel ca in JavaScript, sa marcam absenta sau lipsa initializarii unei variabile folosind tipurile: **null** si **undefined**
- Pentru a marca un atribut ca fiind optional putem folosi operatorul **?**
```typescript
    function printName(obj: { first: string; last?: string }) {
    // ...
    }
    // Both OK
    printName({ first: "Bob" });
    printName({ first: "Alice", last: "Alisson" });
```
- Daca incercam sa accesam o valoare neinitailizata ea va returna **undefined**, asa ca o varianta mai safe ar fi sa folosim tot operatorul **?**
```typescript
    function printName(obj: { first: string; last?: string }) {
    // Error - might crash if 'obj.last' wasn't provided!
    console.log(obj.last.toUpperCase());

    if (obj.last !== undefined) {
        // OK
        console.log(obj.last.toUpperCase());
    }
    
    // A safe alternative using modern JavaScript syntax:
    console.log(obj.last?.toUpperCase());
    }
```
- Pentru a nu mai face verificare de nullable putem folosi operatorul **!**
```typescript
    function liveDangerously(x?: number | null) {
        // No error
        console.log(x!.toFixed());
    }

    //Echivalent cu:
    function liveDangerously(x?: number | null) {
        if(x !== null) {
            console.log(x.toFixed());
        }
    }
```

## Alte resurse faine
- Documentatia oficiala: https://www.typescriptlang.org/docs/handbook/intro.html
- Un intro pentru cei care au mai lucrat cu JS: https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
- Un video super de la Fireship: https://www.youtube.com/watch?v=ahCwqrYpIuM
- Un video care vorbeste de diferenta dintre TS-ul functional si cel OOP oriented: https://www.youtube.com/watch?v=fsVL_xrYO0w