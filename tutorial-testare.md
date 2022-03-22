# Tutorial Testare

## Why testing?
- Testarea are 2 principale efecte pozitive:
  - ajuta la o intelegere mai rapida si mai eficienta a codului
  - ne permite sa testam intr-un mod automat anumite functionalitati
- Astfel, cu cat aplicatia noastra creste mai mult in feature-uri este util sa automatizam procesul de testare
- In plus, aceasta metodologie ne permite creearea unui mediu uniform pentru a testa codul nostru

## Types of testing
- Exista mai multe tipuri de testare, cele mai importante fiind:
  - **Unit testing** - Testarea unei zone mici de cod intr-un mod izolat(nu avem dependinte)
  - **Integration testing** - Testarea mai ampla a interactiunii dintre componentele sistemului nostru
  - **End-to-end testing** - Testarea intregului sistem 
  - **Penetration(security) testing** - Testarea breselor de securitate
- Pe noi, ca developeri, ne intereseaza in mod special testarea unitara si de integrare

## Testarea unitara
- In cazul testelor unitare trebuie sa testam o bucata minimala de cod(o functie de regula) intr-un mod izolat de restul sistemului
- Pentru a reusi sa izolam bucata noastra de cod de alte dependinte avem 2 optiuni: **fake-uri** sau **mock-uri**

## Integration testing
- In cazul testelor de integrare testam mai multe sisteme si modul in care interactioneaza acestea

## Tools and principles

### **Jest**
- **Jest** este libraria care ne va ajuta atat pe frontend, cat si backend sa scriem testele noastre
- Testele noastre au 3 etape: 
  - setup: organizam aplicatia noastra intr-un anumit fel
  - action: realizam operatiunea pe care vrem sa o testam
  - assertion: verificam outcome-ul
- Testele pentru o anumita componenta le vom grupa folosind functia ***describe()***
- Pentru a crea un test vom folosi functia ***test()***
- Pentru a realiza o asertie vom folosi functiile ***expect()*** si ***toBe()***
- Exemplu: 
```typescript
    describe("testele pentru repository-ul nostru", () => {
        test("should return all objects", () => {
            //setup
            const repo = new MyRepo();
            
            //action
            const elemList: Elem[] = repo.getAll();

            //assertion
            experct(elemList.size()).toBe(4);
         });

         test("should return element by id", () => {
             //
            const repo = new MyRepo();

            const elem: Elem = repo.get(10);

            experct(elem).not().toBe(null);
         });
    });
```
- Pentru mai multe functii care ne permit sa realizam asertii: https://jestjs.io/docs/using-matchers


### Mocking
- **Mock**-urile sunt folosite pentru a simula o componenta reala fara a o initializa
- Facem acest lucru din 2 motive:
  - Pentru ca testul nostru unitar trebuie sa testeze izolat o functionalitatea 
  - Pentru a putea monitoriza diferite interactiuni cu componenta mock-uita
- Exemplu: 
```typescript 
    import SoundPlayer from './sound-player';
    import SoundPlayerConsumer from './sound-player-consumer';
    jest.mock('./sound-player'); // SoundPlayer is now a mock constructor

    beforeEach(() => {
    // Clear all instances and calls to constructor and all methods:
    SoundPlayer.mockClear();
    });

    it('We can check if the consumer called the class constructor', () => {
    const soundPlayerConsumer = new SoundPlayerConsumer();
    expect(SoundPlayer).toHaveBeenCalledTimes(1);
    });

    it('We can check if the consumer called a method on the class instance', () => {
    // Show that mockClear() is working:
    expect(SoundPlayer).not.toHaveBeenCalled();

    const soundPlayerConsumer = new SoundPlayerConsumer();
    // Constructor should have been called again:
    expect(SoundPlayer).toHaveBeenCalledTimes(1);

    const coolSoundFileName = 'song.mp3';
    soundPlayerConsumer.playSomethingCool();

    // mock.instances is available with automatic mocks:
    const mockSoundPlayerInstance = SoundPlayer.mock.instances[0];
    const mockPlaySoundFile = mockSoundPlayerInstance.playSoundFile;
    expect(mockPlaySoundFile.mock.calls[0][0]).toEqual(coolSoundFileName);
    // Equivalent to above check:
    expect(mockPlaySoundFile).toHaveBeenCalledWith(coolSoundFileName);
    expect(mockPlaySoundFile).toHaveBeenCalledTimes(1);
});

```
### Supertest
- **Supertest** este o librarie care ne permite sa realizam request-uri catre api-ul nostru si sa testam raspunsul
- Inainte sa folosim aceasta librarie va trebui sa separam instanta noastra de ***express*** de momentul apelului functiei ***listen***
- Astfel vom avea 2 fisiere:
#### **`app.ts`**
```typescript 
    import express, { Application } from 'express';
    import { mainRouter } from './router';

    let app: Application = express();

    app.use(express.json());

    export default app;
```

#### **`server.ts`**
```typescript 
    import app from '../app';

    app.listen(SOME_PORT, () => {
        console.log(`Express is listening at http://localhost:${SOME_PORT}`)
    });
```
- Pentru a trimite un request vom folosi functia ***request()***
```typescript 
    import app from "../../app";
    import request from "supertest";

    describe("POST /register", () => {
        test("returns status code 200 if we successfully register", async () => {
            const res = await request(app)
                        .post("/register")
                        .send({email: "ceva@gmail.com"});
            
            expect(res.statusCode).toBe(200);
        });
    });
```


## Alte resurse faine
- **Documentatia oficiala**(este gen divina): https://jestjs.io/docs/getting-started
- https://medium.com/codeclan/mocking-es-and-commonjs-modules-with-jest-mock-37bbb552da43
- Ceva si cu putin supertest: https://medium.com/@natnael.awel/how-to-setup-testing-for-typescript-with-express-js-example-83d3efbb6fd4
- Un video care vorbeste de diferenta dintre TS-ul functional si cel OOP oriented: https://www.youtube.com/watch?v=fsVL_xrYO0w
- Ceva si pentru frontend: https://www.freecodecamp.org/news/testing-react-hooks/