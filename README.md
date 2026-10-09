# SimpliJS Browser / CDN v0.4.0

SimpliJS is a browser-first programming language layer over JavaScript. This project contains the compiler source and a dependency-free browser bundle intended to be served as a single CDN file.

## Runtime model

The published browser bundle is all the visitor needs. A user's page includes the bundle, the browser finds every `<simplijs>` element, compiles the SimpliJS source locally, and executes the generated JavaScript locally.

There is no SimpliJS backend, no server-side compiler, no Node.js requirement for visitors, and no download/install step for website users.

## Use it

```html
<script src="https://simplijscdn.netlify.app/dist/simplijs.min.js"></script>

<simplijs>
    set name to "Mahad"
    set "#title" inner text to name
</simplijs>
```

## Included language areas

- Variables and arithmetic commands
- English comparisons and control flow
- Functions, arguments and return values
- Native JavaScript object variables
- Readable object creation blocks
- Object properties, `Object.keys`, `Object.values`, `Object.entries`
- Object functions/methods with `this`
- Generic English instance-method bridge for native/custom methods
- Arrays, indexing and basic mutations
- English aliases for `splice`, `slice`, `join`, `includes`, `indexOf`, `at`, `map`, `filter`, `find`, `every`, `some`, `reduce`, and more
- Full native array-method access through the generic method bridge
- JSON-backed localStorage CRUD
- sessionStorage CRUD
- storage key/length/existence helpers
- DOM text, HTML, value, properties, attributes and styles
- Classes, visibility, element creation and child operations
- Selector queries and parent/sibling relationships
- Event listeners with a browser event object
- HTML template interpolation
- Fetch JSON and POST helpers
- Browser navigation shortcuts
- `ask()` and `askYesNo()` user prompts
- Math helpers: `random()`, `randomBetween()`, `round()`, `roundUp()`, `roundDown()`, `absolute()`, `power()`, `squareRoot()`, `min()`, `max()` and `pi()`
- Date/time helpers: `now()`, `today()`, `timeNow()`, `currentYear()`, `dateIn()`, `timeIn()` and `dateTimeIn()`
- Form value, checkbox, validity, validation-message and form-data helpers
- Friendly compiler errors with line-first typo suggestions and browser warnings
- Explicit `javascript:` blocks for arbitrary JavaScript

## Browser API

`dist/simplijs.js` and `dist/simplijs.min.js` are browser-ready artifacts. They expose:

- `SimpliJS.version`
- `SimpliJS.compile(source)`
- `SimpliJS.inspect(source)`
- `SimpliJS.run(source)`
- `SimpliJS.compileDocument()`

## Build and test

Development requires Node.js because the compiler source is authored as ES modules and the repository uses a small custom bundling script.

```bash
npm test
npm run build
```

The build produces `dist/simplijs.js` and `dist/simplijs.min.js`.

## Important browser note

The runtime executes generated JavaScript in the page. As with any client-side code generator, the page's Content Security Policy can restrict dynamic script execution. A nonce on the SimpliJS script tag can be reused by the runtime when appropriate.

## Collections, storage and timers

Arrays can contain objects, including nested objects and arrays:

```simplijs
set users to [
    { name: "Mahad", age: 13 },
    { name: "Ali", age: 18 }
]

set firstUser to item 1 from users
set firstName to property "name" of firstUser
set property "age" of item 1 in users to 14
add { name: "Ahmed", age: 20 } to users
remove item 2 from users
insert { name: "Sara", age: 21 } at 2 in users
```

The same JSON-compatible structures can be saved and changed directly in browser storage:

```simplijs
save users as "users"
add { name: "Ahmed", age: 20 } to saved "users"
set property "age" of item 1 in saved "users" to 14
set item 2 in saved "users" to { name: "Sara", age: 21 }
remove item 3 from saved "users"
set usersAgain to saved "users"
```

Saving the same key is also the update operation:

```simplijs
set saved "users" to usersAgain
```

For async work:

```simplijs
wait(2)

after 2 seconds as timeout:
    show "Done"

every 1 second as interval:
    show timeNow()

clear timer interval
```

`wait(2)` waits two seconds in the current asynchronous SimpliJS flow. `after` maps to `setTimeout`, and `every` maps to `setInterval`.
