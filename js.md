# JavaScript Notes

1. Question 1
```JavaScript
// Function that returns a string representing a cup of green tea
// ==== full arrow function ====
const prepareTea = () => 'greenTea';

// ===== Me Just foolin' - Full normal fxn =====
// const prepareTea = function() {
//   return 'greenTea';
// }

// ===== Me just foolin' (again) - Half normal half arrow fxn =====
// const prepareTea = () => { 'greenTea' }

/*
Given a function (representing the tea type) and number of cups needed, the
following function returns an array of strings (each representing a cup of
a specific type of tea).
*/
const getTea = (numOfCups) => {
  const teaCups = [];


/*
=== Dim teaCups as array
=== Dim teaCup as integer
=== Dim getTea as function
=== Dim prepareTea as function
=== Dim cups as integer
=== Dim numOfCups as integer
=== Dim tea4TeamCC as array
*/

// === for loop | initialise: cups = 1 | condition: cups <= numOfCups | increment: cup + 1 ===
  for(let cups = 1; cups <= numOfCups; cups += 1) {
    const teaCup = prepareTea();
    teaCups.push(teaCup);
  }
  return teaCups;
};

// Only change code below this line
const tea4TeamFCC = getTea(40);
console.log(tea4TeamFCC)
// Only change code above this line
```

2. Question 2
```javascript
// Function that returns a string representing a cup of green tea
const prepareGreenTea = () => 'greenTea';

// Function that returns a string representing a cup of black tea
const prepareBlackTea = () => 'blackTea';

/*
Given a function (representing the tea type) and number of cups needed, the
following function returns an array of strings (each representing a cup of
a specific type of tea).
*/

// getTea = input1:prepareTea(fxn), input2(int):numOfCups
const getTea = (prepareTea, numOfCups) => {
  const teaCups = []; //initialise empty 'teaCups' arrray

  //if cups <= numOfCups, add a cup (+1)
  for(let cups = 1; cups <= numOfCups; cups += 1) {
    // Dim teaCup as integer,
    const teaCup = prepareTea();
    teaCups.push(teaCup);
  }
  return teaCups;
};

// Only change code below this line
// prepareGreenTea and prepareBlackTea are like 'objects'
// so below lines refer to get object 'preparedGreenTea' x 27
// get object 'preparedBlackTea' x 13
const tea4GreenTeamFCC = getTea(prepareGreenTea,27);
const tea4BlackTeamFCC = getTea(prepareBlackTea,13);
// Only change code above this line

console.log(
  tea4GreenTeamFCC,
  tea4BlackTeamFCC
);
```