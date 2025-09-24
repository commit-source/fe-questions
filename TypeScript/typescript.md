## TypeScript - общие вопросы

1. <details>
   <summary>Что такое TypeScript? Какие преимущества даёт TypeScript</summary>

   <b>TypeScript</b> — это язык программирования, разработанный Microsoft как надстройка над JavaScript. Он добавляет статическую типизацию, современные возможности ECMAScript и инструменты для масштабируемой разработки, сохраняя полную совместимость с JavaScript.

   <b>Преимущества TypeScript:</b>
   - Позволяет объявлять типы переменных, параметров функций и возвращаемых значений.
   - Ловит ошибки на этапе компиляции (например, передачу неверного типа данных), а не во время выполнения.
   - Типы служат документацией: код становится понятнее для команды.
   - Рефакторинг проще — IDE подскажет, где что-то сломается после изменений.
   - Интеграция с популярными фреймворками (React, Angular, Vue) через типы (например, @types/react).
   - Компилируется в чистый JavaScript, работающий в любом браузере или на сервере (Node.js).
   - Поддерживает все фичи ES6+/ES2025 (классы, модули, деструктуризация, async/await и др.), даже если целевая среда их не поддерживает (благодаря компиляции).
   - Официальная поддержка от Microsoft, регулярные обновления.

   <b>Минусы TypeScript:</b>
   - Проблемы со сторонними библиотеками. Не все JS-библиотеки имеют качественные типовые определения (@types/...). Иногда приходится писать их самому или использовать any, что снижает надёжность.
   - Типы != гарантия отсутствия ошибок. TypeScript проверяет только типы, но не логику. Например, он не поймает ошибку деления на ноль или неверный алгоритм:
   ```javascript
   function divide(a: number, b: number): number {
    return a / b; // TypeScript не предупредит о возможности b = 0!
   }
   ```
   - Дополнительная сложность: Для работы с TypeScript нужно изучить типизацию и особенности компиляции, что может быть проблемой для начинающих разработчиков или тех, кто уже привык к динамическим языкам, таким как JavaScript.
   </details>

1. <details>
   <summary>Какая сейчас версия TypeScript и в чем отличие от предыдущей?</summary>

   На данный момент актуальная стабильная версия TypeScript — <b>5.5.x</b>

   <b>Различия между TypeScript 4.x и TypeScript 5.x (мажорное обновление, выпущенное в марте 2023 года):</b>
   - TypeScript 5.0 был оптимизирован для скорости компиляции и уменьшения использования памяти.
   - Ускорение компиляции на 5–20% (в зависимости от проекта).
   - Снижение использования памяти (особенно важно для крупных проектов).
   - Более быстрая работа с большими файлами (например, в монолитных приложениях). <i>Пример:</i> В проектах с 100 000+ строк кода время компиляции сократилось с ~10 секунд до ~5–7 секунд.
   </details>

1. <details>
   <summary>В чём отличие type от interface?</summary>

    В TypeScript существуют два способа описания типов: <b>type</b> и <b>interface</b>.

    <b>interface</b> используется для описания структуры объектов и классов, а также для расширения других интерфейсов. Это делает его удобным для работы с объектно-ориентированным подходом.
    <b>type</b> более универсален и может использоваться не только для объектов, но и для других типов данных, таких как примитивы, объединения, пересечения типов и другие.

    Отличия у <b>interface</b>:
    - Используется для описания структуры объектов (классов, функций, индексных сигнатур).
    - Поддерживает расширение (extends) и объединение (декларативное слияние).
    - Может описывать классы (через implements).

    ```javascript
    interface User {
      name: string;
      age: number;
    }

    // Расширение
    interface Admin extends User {
      role: string;
    }

    // Объединение (если объявить дважды — сольются)
    interface User {
      email: string;
    }
    const user: User = { name: "Игорь", age: 30, email: "test@example.com" };
    ```

    Отличие у <b>type</b>:
    - Более гибкий: может описывать любые типы, включая примитивы, юнионы, кортежи, mapped types.
    - Использует оператор = для присваивания.
    - Не поддерживает слияние (повторное объявление вызовет ошибку)Не поддерживает слияние (повторное объявление вызовет ошибку).

    ```javascript
    type ID = string | number; // Юнион
    type Point = [number, number]; // Кортеж
    type User = {
      name: string;
      age: number;
    };

    // Расширение через пересечение (&)
    type Admin = User & { role: string };
    ```
   </details>

1. <details>
   <summary>Что такое индексная сигнатура в TypeScript?</summary>

   <b>Индексная сигнатура в TypeScript</b> — это способ описать динамические свойства объекта, когда заранее неизвестны имена ключей, но известен их тип и тип значений.
   Она позволяет указать, что объект может иметь произвольное количество свойств с определённым типом ключа (обычно string или number) и типом значения.

   Пример:
   ```javascript
    interface StringDictionary {
      [key: string]: number; // Любой ключ типа string, значение — number
    }

    const wordCounts: StringDictionary = {
      apple: 5,
      banana: 10,
      orange: 3,
      // Можно добавлять новые ключи динамически:
      grape: 7,
    };

    wordCounts.apple = "five"; // Ошибка: Type 'string' is not assignable to type 'number'.
   ```
   </details>

1. <details>
   <summary>В чем отличие void, never, any, unknown?</summary>

   <b>void</b> — указывает на отсутствие возвращаемого значения (или undefined в нестрогом режиме).
    Используется для функций, которые ничего не возвращают (или возвращают undefined).

   Пример <b>void</b>:
   ```javascript
    // Функция без return
    function logMessage(message: string): void {
      console.log(message);
      // Неявный return undefined
    }

    // Явный return undefined
    function doNothing(): void {
      return undefined;
    }
   ```

   <b>never</b> — тип, который никогда не наступает. Используется для функций, которые никогда не завершаются (бесконечный цикл, исключение) или для исчерпывающих проверок (например, в конструкции switch).

    Пример <b>never</b>:
    ```javascript
      // 1. Функция всегда бросает ошибку
      function throwError(message: string): never {
        throw new Error(message);
      }

      // 2. Бесконечный цикл
      function infiniteLoop(): never {
        while (true) {}
      }

      // 3. Исчерпывающая проверка типов
      function processValue(value: string | number): void {
        if (typeof value === "string") {
          console.log(value.toUpperCase());
        } else if (typeof value === "number") {
          console.log(value.toFixed(2));
        } else {
          const exhaustiveCheck: never = value; // Ошибка, если добавлен новый тип в union
        }
      }
    ```

    <b>any</b> — позволяет отключить проверку типов. При использовании any TypeScript не будет проверять, какие операции выполняются с этим значением. Это дает полную свободу, но также убирает все преимущества статической типизации, так как компилятор не будет предупреждать о потенциальных ошибках.

    Пример <b>any</b>:
    ```javascript
      let dynamicValue: any = "Hello";
      dynamicValue = 42;          // OK
      dynamicValue = [];          // OK
      dynamicValue.foo();         // OK (ошибка только в runtime!)

      function unsafeParse(json: string): any {
        return JSON.parse(json); // Неизвестная структура
      }
    ```

    <b>unknown</b> — unknown также может содержать значение любого типа, но TypeScript требует, чтобы перед выполнением операций вы проверили или привели тип переменной. Это делает unknown более безопасным типом по сравнению с any, так как позволяет избежать ошибок, связанных с несовместимыми типами.

    Пример <b>unknown</b>:
    ```javascript
      function safeParse(json: string): unknown {
        return JSON.parse(json); // Тип unknown — требует проверки
      }

      const data: unknown = safeParse('{"name": "Игорь"}');

      // Ошибка: Property 'name' does not exist on type 'unknown'.
      // console.log(data.name);

      // ✅ Правильный способ: проверка типа
      if (typeof data === "object" && data && "name" in data) {
        console.log((data as { name: string }).name); // "Игорь"
        // Или лучше:
        const typedData = data as { name: string };
        console.log(typedData.name);
      }
    ```
   </details>

1. <details>
   <summary>Что такое Generic в TypeScript?</summary>

   <b>Generic в TypeScript</b> — это возможность создавать универсальные компоненты и функции, которые могут работать с любыми типами данных, при этом сохраняют типовую безопасность. С помощью Generic можно создать функции, классы и интерфейсы, которые могут работать с разными типами, не теряя при этом строгой типизации.

   <b>Зачем нужны Generic?</b>
   - Гибкость: Generic позволяют создавать универсальные функции и классы, которые могут работать с различными типами данных. Это дает возможность избежать дублирования кода, обеспечивая при этом типовую безопасность.
   - Повторное использование кода: Generic позволяют писать код, который можно многократно использовать для разных типов, улучшая читаемость и уменьшая количество повторений.
   - Типовая безопасность: Использование Generic обеспечивает проверку типов в момент компиляции, что помогает избежать ошибок, связанных с неправильным использованием типов.

   <b>Generic в функциях</b>
   С помощью Generic можно создавать функции, которые принимают параметры разных типов, при этом TypeScript будет отслеживать типы данных.

   Пример:
   ```javascript
    function identity<T>(arg: T): T {
      return arg;
    }

    let output1 = identity<string>("Hello");
    let output2 = identity<number>(100);
   ```
   В этом примере T — это обобщенный тип, который будет автоматически определяться на основе типа переданного аргумента. Функция identity возвращает значение того же типа, что и переданному аргументу.

   <b>Generic с массивами</b>
   Мы можем создавать функции для работы с массивами, где элементы массива могут быть любого типа.

   Пример:
   ```javascript
    function logArray<T>(arr: T[]): void {
      arr.forEach(item => console.log(item));
    }

    logArray([1, 2, 3]);  // number[]
    logArray(["a", "b", "c"]);  // string[]
   ```
   В этом примере функция logArray принимает массив с любыми типами данных и выводит его элементы в консоль.

   <b>Generic в интерфейсах</b>
   Generic могут быть использованы в интерфейсах для создания более гибких и универсальных структур данных.

   Пример:
   ```javascript
    interface Box<T> {
      value: T;
    }

    let box1: Box<string> = { value: "Hello" };
    let box2: Box<number> = { value: 100 };
   ```
   В этом примере <i>Box</i> является универсальным интерфейсом, который может работать с любыми типами, и тип данных будет задаваться при создании экземпляра интерфейса.

   <b>Generic в классах</b>
   Generic также можно использовать в классах для создания универсальных классов.

   Пример:
   ```javascript
    class Box<T> {
      private value: T;

      constructor(value: T) {
        this.value = value;
      }

      getValue(): T {
        return this.value;
      }
    }

    const box1 = new Box<string>("Hello");
    const box2 = new Box<number>(100);

    console.log(box1.getValue());  // "Hello"
    console.log(box2.getValue());  // 100
   ```
   В этом примере класс Box использует Generic для работы с разными типами данных, что позволяет создавать экземпляры с различными типами значений.

   <b>Ограничения Generic</b>
   Можно ограничить типы, которые могут быть использованы в Generic, с помощью ограничений. Например, вы можете указать, что параметр типа должен быть объектом с определенным свойством.

   Пример:
   ```javascript
    function logLength<T extends { length: number }>(arg: T): void {
      console.log(arg.length);
    }

    logLength("Hello");  // 5
    logLength([1, 2, 3]);  // 3
   ```
   Здесь <i>T extends { length: number }</i> указывает, что <i>T</i> должен быть объектом с свойством <i>length</i>, таким образом, функция может работать только с такими типами, как строками или массивами.
   </details>

1. <details>
   <summary>Что такое Union в TypeScript?</summary>

   <b>Union (объединение)</b> в TypeScript позволяет создавать типы, которые могут быть одним из нескольких типов. Это означает, что переменная или параметр функции может иметь несколько возможных типов данных. Union помогает делать код более гибким, обеспечивая возможность работать с несколькими типами данных одновременно.
   Union типы создаются с помощью оператора | (или). Он позволяет указать несколько типов, между которыми можно выбирать.

   Пример:
   ```javascript
   let value: string | number;

   value = "Hello";  // Допустимо
   value = 42;       // Допустимо
   value = true;     // Ошибка, так как тип не является ни string, ни number
   ```
   В этом примере переменная value может быть либо строкой, либо числом. Если присвоить значение другого типа, TypeScript выдаст ошибку.
   </details>

1. <details>
   <summary>Что такое TypeGuard?</summary>

   <b>TypeGuard</b> - это механизм в TypeScript, который помогает сузить тип переменной в пределах блока кода. Он позволяет TypeScript точно определить тип переменной на основе условий, что делает код более безопасным и позволяет компилятору лучше проверять типы.

   <b>Принцип работы TypeGuard.</b>
   TypeScript предоставляет несколько способов реализации TypeGuard, включая:
   - Операторы проверки типа, такие как <i>typeof</i> и <i>instanceof</i>.
   - Пользовательские функции TypeGuard с использованием <i>is</i>.

   <b>Пример использования typeof</b>:
   Оператор <i>typeof</i> позволяет проверять примитивные типы, такие как <i>string</i>, <i>number</i>, <i>boolean</i> и другие. В случае с TypeGuard это позволяет сузить тип переменной в блоке кода.
   ```javascript
    function printLength(value: string | number) {
      if (typeof value === "string") {
        console.log(value.length);  // Работает, так как value точно строка
      } else {
        console.log(value.toFixed(2)); // Работает, так как value точно число
      }
    }

    printLength("Hello");  // Выведет: 5
    printLength(42);       // Выведет: 42.00
   ```
   В этом примере, оператор <i>typeof</i> позволяет TypeScript понять, что в блоке if переменная value является строкой, а в блоке else — числом.

   <b>Пример использования instanceof</b>:
   Оператор <i>instanceof</i> используется для проверки типов объектов, например, классов. Это позволяет точно определить тип объекта, если он является экземпляром какого-то класса.
   ```javascript
    class Dog {
      bark() {
        console.log("Woof!");
      }
    }

    class Cat {
      meow() {
        console.log("Meow!");
      }
    }

    function speak(animal: Dog | Cat) {
      if (animal instanceof Dog) {
        animal.bark();  // Доступ к методу bark, так как animal — это Dog
      } else {
        animal.meow();  // Доступ к методу meow, так как animal — это Cat
      }
    }

    const dog = new Dog();
    const cat = new Cat();

    speak(dog);  // Выведет: Woof!
    speak(cat);  // Выведет: Meow!
   ```
   В этом примере, оператор <i>typeof</i> позволяет TypeScript понять, что в блоке <i>if</i> переменная <i>value</i> является строкой, а в блоке <i>else</i> — числом.

   <b>Пример пользовательской TypeGuard функции</b>:
   Мы можем создавать свои собственные функции для проверки типов и использования TypeGuard с помощью ключевого слова <b>is</b>.
   ```javascript
    type Dog = { bark: () => void };
    type Cat = { meow: () => void };

    function isDog(animal: Dog | Cat): animal is Dog {
      return (animal as Dog).bark !== undefined;
    }

    function speak(animal: Dog | Cat) {
      if (isDog(animal)) {
        animal.bark();  // animal теперь точно тип `Dog`
      } else {
        animal.meow();  // animal теперь точно тип `Cat`
      }
    }

    const dog: Dog = { bark: () => console.log("Woof!") };
    const cat: Cat = { meow: () => console.log("Meow!") };

    speak(dog);  // Выведет: Woof!
    speak(cat);  // Выведет: Meow!
   ```
   </details>
