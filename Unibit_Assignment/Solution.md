```
1 . Given an array of integers and a target value, you must determine which two integers' sum
equals the target and return a 2D array. Then merge the array into a single array with sorting (
ascending ) order, in the next step double the target value and find again the combination of
digits (can be multiple digits ) that are equal to the double targeted value and returned into a 2Darray.

Sample Input :
[1, 3, 2, 2, -4, -6, -2, 8];
Target Value = 4

Output:
First Combination For “4” : [ [1,3],[2,2],[-4,8],[-6,2] ];
Merge Into a single Array : [-6,-4,1,2,2,2,3,8];
Second Combination For “8” : [ [ 1,3,2,2], [8,-4,2,2],....,[n,n,n,n] ]
```

## Solution 1

```js
// Function to find First Combination
function findTwoSum(arr, target) {
  arr.sort();
  let obj = {};
  let res = [];
  arr.forEach((ele) => {
    let t = target - ele;
    if (obj[t]) {
      res.push([obj[t], ele]);
    }

    obj[ele] = ele;
  });
  return res;
}

// Function to find Second Combination
function findTargetSum(arr, index, target, curr, res) {
  if (target === 0) {
    res.push([...curr]);
    return;
  }

  if (target < 0 || index >= arr.length) return;

  curr.push(arr[index]);
  findTargetSum(arr, index + 1, target - arr[index], curr, res);
  curr.pop();
  findTargetSum(arr, index + 1, target, curr, res);
}

// Function to merge 2D Array
function merge2dArr(arr) {
  return arr
    .reduce((acc, ele) => {
      acc.push(...ele);
      return acc;
    }, [])
    .sort();
}

// Helper Function
function findSecondTargetSum(arr, target) {
  let res = [];
  findTargetSum(arr, 0, target, [], res);
  return res;
}

// Main Method
function main() {
  let arr = [1, 3, 2, 2, -4, -6, -2, 8];
  let target = 4;

  let targetTwoSumArray = findTwoSum(arr, target);
  console.log(`First Combination For ${target} :`, targetTwoSumArray);

  let mergedArray = merge2dArr(targetTwoSumArray);
  console.log(`Merged Array :`, mergedArray);

  let secondTargetSumArray = findSecondTargetSum(mergedArray, target * 2);
  console.log(`Second Combination For ${target * 2} :`, secondTargetSumArray);
}

// Calling Main method
main();
```

### Output

```js
First Combination For 4 : [ [ 2, 2 ], [ 1, 3 ], [ -4, 8 ] ]
Merged Array : [ -4, 1, 2, 2, 3, 8 ]
Second Combination For 8 : [ [ -4, 1, 3, 8 ], [ -4, 2, 2, 8 ], [ 1, 2, 2, 3 ], [ 8 ] ]

```

## Solution 2 (Sum === Mod of Target)

```js
// Function to find First Combination
function findTwoSum(arr, target) {
  arr.sort();
  let res = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (Math.abs(arr[i] + arr[j]) === target) {
        res.push([arr[i], arr[j]]);
      }
      while (j + 1 < arr.length && arr[j] === arr[j + 1]) j++;
    }
    while (i + 1 < arr.length && arr[i] === arr[i + 1]) i++;
  }

  return res;
}

// Function to find Second Combination
function findTargetSum(arr, index, target, curr, res) {
  if (target === 0) {
    res.push([...curr]);
    return;
  }

  if (target < 0 || index >= arr.length) return;

  curr.push(arr[index]);
  findTargetSum(arr, index + 1, target - arr[index], curr, res);
  curr.pop();
  findTargetSum(arr, index + 1, target, curr, res);
}

// Function to merge 2D Array
function merge2dArr(arr) {
  return arr
    .reduce((acc, ele) => {
      acc.push(...ele);
      return acc;
    }, [])
    .sort();
}

// Helper Function
function findSecondTargetSum(arr, target) {
  let res = [];
  findTargetSum(arr, 0, target, [], res);
  return res;
}

// Main Method
function main() {
  let arr = [1, 3, 2, 2, -4, -6, -2, 8];
  let target = 4;

  let targetTwoSumArray = findTwoSum(arr, target);
  console.log(`First Combination For ${target} :`, targetTwoSumArray);

  let mergedArray = merge2dArr(targetTwoSumArray);
  console.log(`Merged Array :`, mergedArray);

  let secondTargetSumArray = findSecondTargetSum(mergedArray, target * 2);
  console.log(`Second Combination For ${target * 2} :`, secondTargetSumArray);
}

// Calling Main method
main();
```

### Output

```js
First Combination For 4 : [ [ -4, 8 ], [ -6, 2 ], [ 1, 3 ], [ 2, 2 ] ]
Merged Array : [
  -4, -6, 1, 2,
   2,  2, 3, 8
]
Second Combination For 8 : [
  [
    -4, -6, 1, 2,
     2,  2, 3, 8
  ],
  [ -4, 1, 3, 8 ],
  [ -4, 2, 2, 8 ],
  [ -4, 2, 2, 8 ],
  [ -4, 2, 2, 8 ],
  [ -6, 1, 2, 3, 8 ],
  [ -6, 1, 2, 3, 8 ],
  [ -6, 1, 2, 3, 8 ],
  [ -6, 2, 2, 2, 8 ],
  [ 1, 2, 2, 3 ],
  [ 1, 2, 2, 3 ],
  [ 1, 2, 2, 3 ],
  [ 8 ]
]

```
