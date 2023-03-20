## Introduction

On this post, I'll explain and help you **understand** in **detail** how to solve every **_FreeCodeCamp JavaScript Algorithms and Data Structures Project_** in a simple way. I have found that many of the code explanations I came across are somewhat weird or complicated, especially for **problems** `2` and `5`.

## Table Of Contents

1. [Problem N°1 : Palindrome Checker](#problem-n%C2%B01-palindrome-checker)

2. [Problem N°2 : Roman Numeral Converter](#problem-n%C2%B02-roman-numeral-converter)

3. [Problem N°3 : Caesars Cipher](#problem-n%C2%B03-caesars-cipher)

4. [Problem N°4 : Telephone Number Validator](#problem-n%C2%B04-telephone-number-validator)

5. [Problem N°5 : Cash Register](#problem-n%C2%B05-cash-register)

---

## [Problem N°1 : Palindrome Checker](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/javascript-algorithms-and-data-structures-projects/palindrome-checker)

{% collapsible **DESCRIPTION** %}

Return `true` if the given string is a palindrome. Otherwise, return `false`.

A _palindrome_ is a word or sentence that's spelled the same way both forward and backward, ignoring punctuation, case, and spacing.

**Note**: You'll need to remove **all non-alphanumeric characters** (punctuation, spaces and symbols) and turn everything into the same case (lower or upper case) in order to check for palindromes.

We'll pass strings with varying formats, such as `racecar`, `RaceCar`, and `race CAR` among others.

We'll also pass strings with special symbols, such as `2A3*3a2`, `2A3 3a2`, and `2_A3*3#A2`.

{% endcollapsible %}

### Solution ✅

```javascript
function palindrome(str) {
  const filteredStr = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  const reversedStr = filteredStr.split("").reverse().join("");
  return filteredStr === reversedStr;
}
palindrome("eye");
```

### Explanation ℹ

This problem is direct and simple :

1. The first thing to do is to convert our string to `lower case` using `toLowerCase()` method.

   - We need to because `"eyE" !== "Eye"`

   ```javascript
   console.log("TesT".toLowerCase());
   //output : "test"
   ```

2. After that we filter **any non-alphanumeric** characters with the help of `regEx` and `replace()` method.

   ```javascript
   console.log("_test55^".replace(/[^a-z0-9]/g, ""));
   //output : "test55"
   ```

3. Now we need to reverse our string so we can compare it to the original :

   - `split()` our string so we have an array of strings.
   - `reverse()` the array.
   - `join()` to return a string from the reversed array.

   ```javascript
   let str = "happy";
   console.log(str.split(""));
   //output : [ 'h', 'a', 'p', 'p', 'y' ]
   console.log(str.split("").reverse());
   //output : [ 'y', 'p', 'p', 'a', 'h' ]
   console.log(str.split("").reverse().join(""));
   //output : 'yppah'
   ```

4. The last step is to compare the original string to the reversed one, if returned `true`, the latter is a **palindrome**

```javascript
return str === strReversed;
```

---

## [Problem N°2 : Roman Numeral Converter](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/javascript-algorithms-and-data-structures-projects/roman-numeral-converter)

{% collapsible **DESCRIPTION** %}

Convert the given number into a roman numeral.

![roman table](https://i.ibb.co/TPSWV59/roman.jpg)

{% endcollapsible %}

### Solution ✅

```javascript
const table = {
  M: 1000,
  CM: 900,
  D: 500,
  CD: 400,
  C: 100,
  XC: 90,
  L: 50,
  XL: 40,
  X: 10,
  IX: 9,
  V: 5,
  IV: 4,
  I: 1,
};

function convertToRoman(num) {
  let roman = "";
  for (const key in table) {
    const value = table[key]; // ex 1000
    while (num >= value) {
      num -= value;
      roman += key;
    }
  }
  return roman;
}

convertToRoman(1234);
```

### Explanation ℹ

This problem is very simple if you just know the **`rules`** to follow.

To convert a **number** to its **Roman** equivalent, you need to do some basic maths with the help of the table above.

Let's imagine we have `number = 999`

1. From the table _(cf. Problem description)_, we search for the number that's (`≥`) greater than or equal to `999`.
   - This number would be `900`.
2. We subtract `999` from `900` and **save the Roman letter**.
   - On this case, we save : `CM`
3. We repeat the same procedure but with the **result of the subtraction**.
   - The result is : `99`

```javascript
999 >= 900, so :
we sub : 999 - 900, save CM,
----
99 >= 90, so :
we sub : 99 - 90, save XC,
----
9 >= 9, so
we sub : 9 - 9, save IX
----
Result : CMXCIX
```

#### Full Examples

![numeral to roman](https://i.ibb.co/kmWkFSD/roman.webp)

---

✳ Now, let's write some JavaScript.

1. We need to create an `object` starting from the table above.

   ```javascript
   const table = {
     M: 1000,
     CM: 900,
     D: 500,
     CD: 400,
     C: 100,
     XC: 90,
     L: 50,
     XL: 40,
     X: 10,
     IX: 9,
     V: 5,
     IV: 4,
     I: 1,
   };
   ```

2. After that we need an empty `string` so we can save our `Roman` letters to it.

   ```javascript
   let roman = "";
   ```

3. And finally we loop thought the `object` with the help of `for in` and :

   - While `number >= (object value)` we subtract `number - (object value)`, and save the `object key` to the empty string we just created.

   ```javascript
   for (const key in table) { // We loop
   const value = table[key]; // (Object value), ex : 1000
   while (num >= value) { // number >= (Object value)
   num -= value; // number - (Object value)
   roman += key; // saving the (Object key), ex : IX
   ```

   - **⚠ Note :**

   ```javascript
   object = {
     name: "Doe", // name is the Object `key` , "Doe" is the Object `value`
   };
   ```

Pretty simple huh ?

---

## [Problem N°3 : Caesars Cipher](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/javascript-algorithms-and-data-structures-projects/caesars-cipher)

{% collapsible **DESCRIPTION** %}

One of the simplest and most widely known _ciphers_ is a _Caesar cipher_, also known as a shift cipher. In a shift cipher the meanings of the letters are shifted by some set amount.

A common modern use is the **`ROT13`** cipher, where the values of the letters are shifted by 13 places. Thus `A ↔ N`, `B ↔ O` and so on.

Write a function which takes a **`ROT13`** encoded string as input and returns a decoded string.

All letters will be uppercase. Do not transform any non-alphabetic character (i.e. spaces, punctuation), but do pass them on.

{% endcollapsible %}

### Solution ✅

```javascript
function rot13(str) {
  const max = 90; // decimal value of Z
  return str
    .split("")
    .map((el) => {
      if (el.match(/[A-Z]/g)) {
        let charCode = el.charCodeAt() + 13;
        return charCode > max
          ? String.fromCharCode((charCode -= 26))
          : String.fromCharCode(charCode);
      } else {
        return el;
      }
    })
    .join("");
}

rot13("SERR PBQR PNZC");
```

### Explanation ℹ

To decode `ROT13` encoded strings, we need to move every letter by 13 places.

![alphabet](https://i.ibb.co/Km6ZPDL/download.png)

```javascript
ROT13(a) = n
// 1 + 13 = 14, from the table, 14 is n
ROT13(c) = p
// 3 + 13 = 16, from the table, 16 is p
```

❓ What if the letter is for example `w`?

```javascript
23 + 13 = 36 // no letter is 36th of the alphabet
```

- We exceeded the number of the English alphabet letters, the **maximum** is `26`.

ℹ To **solve** this, every time the total is greater (`>`) than `26`, we need to **subtract** `26` so we **start** counting from the **start** again.

![rot13 alphabet](https://i.ibb.co/WyPWnfF/rot13-alphabet.webp)

```javascript
ROT13(w) = j
//23 + 13 = 36 ➡ (36>26)
//36 - 26 = 10, from the table, 10 is j
```

**Thats it.**

---

✳ How can we do that with `JavaScript` ?

- In `JavaScript` we have a `method` that returns an integer representing the UTF-16 code unit of a string.

  ```javascript
  console.log("a".charCodeAt()); // 97
  console.log("A".charCodeAt()); // 65
  console.log("z".charCodeAt()); // 122
  console.log("Z".charCodeAt()); // 90
  ```

- Unlike the English alphabet, `A` is number `65`, and `Z` is number `90`.

**⚠ Note :** All letters will be **uppercase** [Cf. Problem description].

- So, like we explained above, to decode `ROT13` encoded strings, we need to move every letter by `13` places, but this time we won't follow the alphabet table, but this table :

  ![ascii table](https://i.ibb.co/2jKP5zv/ascii-table.webp)

Like mentioned above, `A` is `65`, and `Z` is `90`.

- Now, because we are using the `charCodeAt()` method, we need to **convert** back the result to a string using `String.fromCharCode()` method.

  ```javascript
  let charCode = "A".charCodeAt() + 13;
  console.log(charCode);
  // output : 78
  let str = String.fromCharCode(charCode);
  console.log(str);
  // output : N
  ```

❓ What if the letter is for example `W`?

ℹ Like before, if the number after adding `13` is greater (`>`) than the decimal value of `Z` (in this case `90`), we **subtract** `26` (_the total number the English Alphabet_) so we **start counting** from the start again.

![rot13 ascii table](https://i.ibb.co/gmq6Gvx/rot13-ascii-table.webp)

```javascript
let charCode = "W".charCodeAt() + 13;
console.log(charCode);
// output : 100 > 90, so we subtract 26 :
charCode -= 26;
let str = String.fromCharCode(charCode);
console.log(str);
// output : J
```

✳ Now, lemme explain my code a lil bit.

1. First, we `split()` our string so we have an array of strings, after that we `map()` this array.

2. We convert each element of this array to its **decimal** equivalent using `charCodeAt()` and we add **`13`** to it if the latter is **alphabetic**, if not, we `return` it without touching it [Cf. problem description].

3. If after adding `13` the result exceeds the decimal value of `Z`, we subtract `26`.

4. We convert back to a string with the help of `String.fromCharCode()` method.

---

## [Problem N°4 : Telephone Number Validator](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/javascript-algorithms-and-data-structures-projects/telephone-number-validator)

{% collapsible **DESCRIPTION** %}

Return `true` if the passed string looks like a valid US phone number.

The user may fill out the form field any way they choose as long as it has the format of a valid US number. The following are examples of valid formats for US numbers (refer to the tests below for other variants):

```
555-555-5555
(555)555-5555
(555) 555-5555
555 555 5555
5555555555
1 555 555 5555
```

For this challenge you will be presented with a string such as `800-692-7753` or `8oo-six427676;laskdjf`. Your job is to validate or reject the US phone number based on any combination of the formats provided above. The area code is required. If the country code is provided, you must confirm that the country code is `1`. Return `true` if the string is a valid US phone number; otherwise return `false`.

{% endcollapsible %}

### Solution ✅

```javascript
function telephoneCheck(str) {
  const reg = /^(1\s?)?(\d{3}|\(\d{3}\))[\s\-]?\d{3}[\s\-]?\d{4}$/gm;
  return reg.test(str);
}

telephoneCheck("1 555-555-5555");
```

### Explanation ℹ

Explaining `Regular expressions` is another topic beyond this post.

---

## [Problem N°5 : Cash Register](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/javascript-algorithms-and-data-structures-projects/cash-register)

{% collapsible **DESCRIPTION** %}

Design a cash register drawer function `checkCashRegister()` that accepts purchase price as the first argument (`price`), payment as the second argument (`cash`), and cash-in-drawer (`cid`) as the third argument.

`cid` is a 2D array listing available currency.

The `checkCashRegister()` function should always return an object with a `status` key and a `change` key.

Return` {status: "INSUFFICIENT_FUNDS", change: []}` if cash-in-drawer is less than the change due, or if you cannot return the exact change.

Return `{status: "CLOSED", change: [...]}` with cash-in-drawer as the value for the key `change` if it is equal to the change due.

Otherwise, return `{status: "OPEN", change: [...]}`, with the change due in coins and bills, sorted in highest to lowest order, as the value of the `change` key.

| Currency Unit       | Amount             |
| ------------------- | ------------------ |
| Penny               | $0.01 (PENNY)      |
| Nickel              | $0.05 (NICKEL)     |
| Dime                | $0.1 (DIME)        |
| Quarter             | $0.25 (QUARTER)    |
| Dollar              | $1 (ONE)           |
| Five Dollars        | $5 (FIVE)          |
| Ten Dollars         | $10 (TEN)          |
| Twenty Dollars      | $20 (TWENTY)       |
| One-hundred Dollars | $100 (ONE HUNDRED) |

See below for an example of a cash-in-drawer array:

```javascript
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100],
];
```

{% endcollapsible %}

### Solution ✅

```javascript
const rate = {
  PENNY: 0.01,
  NICKEL: 0.05,
  DIME: 0.1,
  QUARTER: 0.25,
  ONE: 1,
  FIVE: 5,
  TEN: 10,
  TWENTY: 20,
  "ONE HUNDRED": 100,
};

function checkCashRegister(price, cash, cid) {
  const totalCid = Number(cid.reduce((sum, el) => sum + el[1], 0).toFixed(2));
  let change = Number((cash - price).toFixed(2));

  if (totalCid === change) {
    return { status: "CLOSED", change: cid };
  } else if (totalCid < change) {
    return { status: "INSUFFICIENT_FUNDS", change: [] };
  } else {
    let changeArray = [];
    cid.reverse().forEach((el) => {
      const coinName = el[0];
      const coinTotalValueInDollars = Number(el[1]);
      const selectedCurrency = Number(rate[coinName]);
      let coinsAvailable = Number(
        (coinTotalValueInDollars / selectedCurrency).toFixed(2)
      );
      let coinsToReturn = 0;
      while (change >= selectedCurrency && coinsAvailable > 0) {
        change = Number((change - selectedCurrency).toFixed(2));
        --coinsAvailable;
        ++coinsToReturn;
      }
      if (coinsToReturn > 0) {
        changeArray.push([coinName, coinsToReturn * selectedCurrency]);
      }
    });
    if (change === 0) {
      return { status: "OPEN", change: changeArray };
    } else {
      return { status: "INSUFFICIENT_FUNDS", change: [] };
    }
  }
}

checkCashRegister(3.26, 100, [
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100],
]);
```

### Explanation ℹ

Ahh! The famous _`Cash Register`_ problem. It's actually quite easy to understand once you grasp it correctly. Trust me, once you get it, you'll see how straightforward it is.

✳ First of all, I need to explain to you the `cash-in-drawer array`

```javascript
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100],
];
```

- Take a look at the `PENNY` element. `1.01` is the **total value** in dollars worth of `PENNIES`, in other words, the drawer have `$1.01` worth of pennies, this means we have `101 PENNY`, because every `PENNY` is worth `$0.01`

- We also have `$2.05` worth of `NICKELS`, this means the drawer have `41 NICKEL`, because every `NICKEL` is worth `$0.05`, and so on...

✳ That in mind, let's have a **real example** first so we can move forward.

Imagine you buy something that costs `$5`, and you give the seller `$20`. The **change** would be `$15`.

Technically, the seller could return the change amount in `dimes`, `nickels`, or `pennies` if they have enough, but ideally they would return **one $10 bill** and **one $5 bill** (_from the highest value bill to the lowest_).

---

✳ Now, let's write some JavaScript.

1. We need first to calculate the change.

   - The change is the amount of `cash` the buyer gave minus the `price` of the purchased item.

   ```javascript
   let change = Number((cash - price).toFixed(2));
   // we limited the decimals to two, using toFixed() method, it will also round the number
   // ex `5.1799.toFixed(2)` will output : 5.18
   // and converted the result to a number using `Number()` method
   ```

2. We need to calculate the total value of the money we have the drawer in dollars.

   ```javascript
   const totalCid = Number(cid.reduce((sum, el) => sum + el[1], 0).toFixed(2));
   // cid is the `cash-in-drawer array`
   // to understand the `reduce()` method, watch : https://youtu.be/g1C40tDP0Bk
   ```

3. We need to know the value or the equivalent of each bill/coin in dollars.

   ```javascript
   const rate = {
     PENNY: 0.01, // this means a PENNY is worth $0.01
     NICKEL: 0.05,
     DIME: 0.1,
     QUARTER: 0.25,
     ONE: 1,
     FIVE: 5,
     TEN: 10,
     TWENTY: 20,
     "ONE HUNDRED": 100,
   };
   ```

4. Now, we need to check :

   - If `totalCid === change`, we return `CLOSED`.
   - If `totalCid < change`, we return `INSUFFICIENT_FUNDS`.
   - If `totalCid > change`, we return `OPEN` and **calculate** the correct amount of the change to be returned.

   ```javascript
   if (totalCid === change) {
     return { status: "CLOSED", change: cid };
   } else if (totalCid < change) {
     return { status: "INSUFFICIENT_FUNDS", change: [] };
   } else {
     // here, we calculate the correct amount of the change to be returned.
     // check step 5
   }
   ```

5. Now, inside the last **else** block, we need to :

   - Create a variable in which we will store the change to return.
   - Reverse the `cid` **array** so that it is in **descending order** (_from the highest value to the lowest_). See the real example I explained above for why this is necessary.
   - Loop the array.

     ```javascript
     let changeArray = [];
     cid.reverse().forEach((el) => {
       // check step 6
     });
     ```

6. Now inside the loop, for each element of the array, we set some variables for :

   - The name of the coin, for example `PENNY`.
   - The total value of the coins we have in the drawer in dollars, for example `1.01`.
   - The value or the equivalent of the coin in dollars, we pick it from the rates object we created in **step 3**, for example `20`.
   - How many coins I have available, for example we have `$1.01` worth of `PENNIES`, and each `PENNY` is worth `$0.01`, this means we have `101` `PENNY`.

     ```javascript
     cid.reverse().forEach((el) => {
       const coinName = el[0]; // PENNY
       const coinTotalValueInDollars = Number(el[1]); // 1.01
       const selectedCurrency = Number(rate[coinName]);
       // rate['PENNY'] -> output : 0.01
       let coinsAvailable = Number(
         (coinTotalValueInDollars / selectedCurrency).toFixed(2)
       );
       // 1.01 / 0.01 -> output : 101
     });
     ```

---

✳ Before continuing, let's first have another real example on how we calculate the right amount of change to be returned using the following `cash-in-drawer array` :

```javascript
[
  ["PENNY", 1.01],
  ["NICKEL", 2.05],
  ["DIME", 3.1],
  ["QUARTER", 4.25],
  ["ONE", 90],
  ["FIVE", 55],
  ["TEN", 20],
  ["TWENTY", 60],
  ["ONE HUNDRED", 100],
];
```

- From the array, and with the help of the rates _object_, we have in the drawer :

| Currency    | Number of bills/coins |
| ----------- | --------------------- |
| ONE HUNDRED | 1                     |
| TWENTY      | 3                     |
| TEN         | 2                     |
| FIVE        | 11                    |
| ONE         | 90                    |
| QUARTER     | 17                    |
| DIME        | 31                    |
| NICKEL      | 41                    |
| PENNY       | 101                   |

ℹ You read the table this way : We have **3** twenty-dollar bills

![fcc Cash Register](https://i.ibb.co/PGTnpyq/money.webp)

- Now, if you buy something worth `$9.50` and you give the seller `$100`, the change would be `$90.5`. To calculate the amount to return :

  - From the currencies we have in the cash drawer (picture above), we search for the one that's (`≤`) less than or equal to the change :
    - Can't be `$100` because `$90.5` > `$100`.
    - The next currency is `$20`, and that's the right one.
  - If we have enough bills/coins of the selected currency, we subtract, if not, we move to the next currency and so on.

![fcc Cash Register](https://i.ibb.co/zZRnVfm/cash.webp)

---

Now, to implement this with code, we use the `while` statement :

- While **change** `≥` **selected currency** `and` we have **enough** bills/coins, we subtract and **save** how many bills/coins we gave.

```javascript
let coinsToReturn = 0; // we didn't give any bills/coins yet
while (change >= selectedCurrency && coinsAvailable > 0) {
  change = Number((change - selectedCurrency).toFixed(2));
  --coinsAvailable; // decrement the total by 1 (-1)
  ++coinsToReturn; // increment the total by 1 (+1)
}
```

- If `coinsToReturn > 0`, it means we gave some bills/coins, so we push to the empty array we created on **step 5** the **currency name** and the **total** in dollars we gave worth of this currency.

```javascript
if (coinsToReturn > 0) {
  changeArray.push([coinName, coinsToReturn * selectedCurrency]);
}
```

- Finally, we need to check, if `change = 0`, this means we can return the amount of change, if not, we have insufficient funds.

```javascript
if (change === 0) {
  return { status: "OPEN", change: changeArray };
} else {
  return { status: "INSUFFICIENT_FUNDS", change: [] };
}
```

**✅ Thats it, we finished 🎉🎉.**

**Note**: I have tried to make this as beginner-friendly as possible, which may result in some repetition and potentially boring sections.

---

You can check the solutions on my github.

[![Github](https://img.shields.io/badge/github-view%20on%20github-blue?style=for-the-badge&logo=github)](https://github.com/aBrihoum/FreeCodeCamp-Projects)
