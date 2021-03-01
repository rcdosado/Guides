Javascript Guides
========

## Unit Tests

### testing a function
1. `npm init` to create package.json
2. Install jest `npm i jest --save-dev` or `npm i jest-cli -g` for global install
3. example of function to test, save as index.js
```javascript
const englishCode = "en-US";
const spanishCode = "es-ES";
function getAboutUsLink(language){
    switch (language.toLowerCase()){
      case englishCode.toLowerCase():
        return '/about-us';
      case spanishCode.toLowerCase():
        return '/acerca-de';
    }
    return '';
}
module.exports = getAboutUsLink;
```
4. create the test as index.test.js

   ```javascript
   const getAboutUsLink = require("./index");
   test("Returns about-us for english language", () => {
       expect(getAboutUsLink("en-US")).toBe("/about-us");
   });
   ```

   

5. run the test simply type `jest`, inside the directory of your test

6. output something like this:
    ```cmd
     PASS  ./index.test.js
      √ Returns about-us for english language (4ms)
      console.log index.js:15
        /about-us
    Test Suites: 1 passed, 1 total
    Tests:       1 passed, 1 total
    Snapshots:   0 total
    Time:        2.389s
    ```

### Mocking a function

we have a function that imports external function
   ```javascript
import { UserStore } from './UserStore'
function getUserDisplayName(){
  const user = UserStore.getUser(userId);
  return `${user.LastName}, ${user.FirstName}`;
}
   ```

the imported function looks like this:
```javascript
 class User {
      getUser(userId){
            // logic to get data from a database
       }
       setUser(user){
             // logic to store data in a database
       }
 }
 let UserStore = new User();
 export { UserStore }
```
Question: how to mock these functions in jest?
1. mock UserStore
    ```javascript
    jest.mock('./UserStore', () => ({
        UserStore: ({
            getUser: jest.fn().mockImplementation(arg => ({
                FirstName: 'Ondrej',
                LastName: 'Polesny'
            })),
            setUser: jest.fn()
        })
    }));
    ```

2. create the test:

   ``` javascript
   test("Returns display name", () => {
       expect(getUserDisplayName(1)).toBe("Polesny, Ondrej");
   })
   ```

3. create a coverage:
``` cmd
jest --coverage
```