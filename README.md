# dateit

A typescript package for converting the fields in your objects to strings.

## Why use dateit?
Let's say you're requesting data from another service, and you want to convert all relevant fields to dates from strings, before manipulating the data or saving it in your database. That's where dateit comes in! It will iterate over the entire object and convert all relevant fields from strings to dates.

## install using:
`> npm install datify`

## How to use dateit
```ts
import dateit from dateit;
```

## Behaviours

### Non convertable strings
Don't worry, dateit will keep them as strings:

```ts
const a = dateit("My regular string!");
// a = "My regular string!"
```

### Four digit number strings
By default they'll be kept as strings, but If you want them to be converted to dates you can set the ```convertFourDigitNumbers``` option to ```true```.
```ts
const a = dateit("1977");
const b = dateit("1977", { convertFourDigitNumbers: true });
// a = "1977", b = new Date("1977")
```

### Numbers
dateit will keep them as numbers.

```ts
const a = dateit(456);
const b = dateit(5678.8);
const c = dateit(1977);
// a = 456, b = 5678.8, c = 1977 (by default), d = new Date("1977")
```

### Booleans
dateit will keep them as booleans.

```ts
const a = dateit(true);
const b = dateit(false);
// a = true, b = false
```

### ISO date strings
By default, dateit will convert all ISO date strings into dates only if they're full ISO dates.

```ts
const a = dateit("2024-12-01T15:25:55.567Z");
// a = new Date("2024-12-01T15:25:55.567Z")
```

if the year/month/day exceed their maximum respectively (for example ```2024-23-01T15:25:55.567Z``` or ```2024-12-40T15:25:55.567Z```), dateit will keep them as strings:
```ts
const a = dateit("2024-30-01T15:25:55.567Z");
// a = "2024-30-01T15:25:55.567Z"
```

dateit will convert ISO dates which are missing miliseconds:
```ts
const a = dateit("2024-06-01T15:25:55Z");
// a = new Date("2024-06-01T15:25:55Z")
```

### Non full ISO date strings
If you want to convert them to dates as well, set the ```convertNonFullIsoDates``` to ```true```. By default, dateit will keep them as strings.

```ts
const a = dateit("1945-04-23");
const b = dateit("1945-04-23", { convertNonFullIsoDates: true });
// a = "1945-04-23", b = new Date("1945-04-23");
```

### JS short date strings
By default, dateit will keep them as strungs, but if you want to convert them as well just set the ```convertShortDates``` option to ```true```.

```ts
const a = dateit("03/25/2015");
const b = dateit("03/25/2015", { convertShortDates: true });
// a = "03/25/2015", b = new Date("03/25/2015");
```

### JS long date strings
By default, dateit will keep them as strungs, but if you want to convert them as well just set the ```convertLongDates``` option to ```true```.

```ts
const a = dateit("May 3 1869");
const b = dateit("May 3 1869", { convertLongDates: true });
// a = "May 3 1869", b = new Date("May 3 1869");
```

### Objects
For each value in the object, dateit will implement the mentioned above behaviours and return the new object.

```ts
const objToConvert = {
    name: "my name",
    age: 12,
    birthDay: "2024-12-01T15:25:55.567Z",
    kids: {
        son: "name",
        daughter: "03/25/2015"
    }
};
const convertedObject = dateit(objToConvert, { convertShortDates: true });
/* convertedObject = {
    name: "my name",
    age: 12,
    birthDay: new Date("2024-12-01T15:25:55.567Z"),
    kids: {
    son: "name",
    daughter: new Date("03/25/2015"),
    }
}
*/
```
***Note: If the ```convertShortDates``` was set to ```false``` here, The field ```objToConvert.kids.daughter``` would have remained a string.***

dateit will access every field and nested field in order to ensure everythin relevant is converted.

### Arrays
The behaviour dateit implements for arrays is identical to the behaviour twords objects.

```ts
const arr = [
    "2024-06-15",
    34,
    "Feb 2 1977",
    false,
    "Hello world",
    "2008-02",
];
const a = dateit(arr, { convertLongDates: true, convertNonFullIsoDates: true });
/* a = [
    new Date("2024-06-15"),
    34,
    new Date("Feb 2 1977"),
    false,
    "Hello world",
    new Date("2008-02"),  
]
*/
```
***Note: If the ```convertLongDates``` and the ```convertNonFullIsoDates``` were set to ```false``` here, all the elements that were converted to dates would have remained strings.***


## Config
The following configuration options for dateit are:
- convertShortDates (false by default) - If you want dateit to convert JS short date strings to dates, set to true.
- convertLongDates (false by default) - If you want dateit to convert JS long date strings to dates, set to true.
- convertNonFullIsoDates (false by default) - If you want dateit to convert non full ISO date strings to dates, set to true.
- convertFourFigitNumbers (false by default) - If you want dateit to convert four figit number strings to dates, set to true.

## Tests
In order to run the tests, clone the repo to your computer and run ```npm test```.

## Issues
If there are any issues, just open an issue in the repository and the owners will fix them.
