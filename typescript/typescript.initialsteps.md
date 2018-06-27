# Typescript Initial Notes

* Based on https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html
* Installing TypeScript
```bash
npm install -g typescript
```
* In your editor, type the following JavaScript code in greeter.ts
```ts
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
* run the TypeScript compiler:
```bash
tsc greeter.ts
```