# callbacks

- this means using function itself as an argument/input. eg->

```javascript
function square(n) {
    return n * n;
}
function cube(n) {
    return n * n * n;
}
function sumOfSquares(a, b) {
    let square1 = square(a);
    let square2 = square(b);
    return square1 + square2;
}
function sumOfCube(a, b) {
    let square1 = cube(a);
    let square2 = cube(b);
    return square1 + square2;
}
let ans=sumOfCube(1, 2);
console.log(ans);
```

- both seems to be similar and repeating, instead of sumofsquare or sumof cubes creating sumofsomtheing function->

```javascript
function someofsomething(a,b,fn){
    let square1 = fn(a);
    let square2 = fn(b);
    return square1 + square2;  
}
let ans sumOfCube(1, 2,square);
console.log(ans)
```

# promises

- better way to write async functions, you can write your own async function but under the hood other prebuilt async functions, you just wrap around other async function to create something new. ex->

```javascript
const fs=require("fs");
function myown(cb){
    fs.readFile("a.txt", "utf-8", function(err, data){
    console.log(err);
    console.log(data);
    data=data + " its me."; fs.writeFile(a.txt, function(){ cb(); })
});
}
myown(function(){
    console.log("created my own sync")
})
```

- in promisified functions you dont take callback function as input, and return a promise when execution completed. its just like a class in javascript, syntax->

```javascript
function promisifiedMyOwnSetTimeout(duration) {
    const p = new Promise(function(resolve) {
    setTimeout(resolve, duration);
    });
    return p;
}
// promise
const ans = promisifiedMyOwnSetTimeout(1000);
ans.then(function() {
console.log("timeout is done");
});

const promiseOne = new Promise(function(resolve, reject){
//Do an async task
// DB calls, cryptography, network
    setTimeout(function(){
    console.log('Async task is compelete');
    resolve()
    }, 1000)
})

promiseOne.then(function(){
console.log("Promise consumed");
})

new Promise(function(resolve, reject){
    setTimeout(function(){
    console.log("Async task 2");
    resolve()
    }, 1000)
}).then(function(){
console.log("Async 2 resolved");
})

const promiseThree = new Promise(function(resolve, reject){
    setTimeout(function(){
     resolve({username: "Chai", email: "[chai@example.com](mailto:chai@example.com)"})
    }, 1000)
})

promiseThree.then(function(user){
    console.log(user);
})

const promiseFour = new Promise(function(resolve, reject){
    setTimeout(function(){
        let error = true
        if (!error) {
            resolve({username: "hitesh", password: "123"})
        } else {
            reject('ERROR: Something went wrong')
        }
        }, 1000)
})

promiseFour
.then((user) => {
    console.log(user);
    return user.username
}).then((username) => {
    console.log(username);
}).catch(function(error){
    console.log(error);
}).finally(() => console.log("The promise is either resolved or rejected"))

const promiseFive = new Promise(function(resolve, reject){
    setTimeout(function(){
    let error = true
    if (!error) {
        resolve({username: "javascript", password: "123"})
    } else {
        reject('ERROR: JS went wrong')
    }
    }, 1000)
});

async function consumePromiseFive(){
try {
    const response = await promiseFive
    console.log(response);
    } catch (error) {
        console.log(error);
}}

consumePromiseFive()

// async function getAllUsers(){
//     try {
//         const response = await fetch('[https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)')

//         const data = await response.json()
//         console.log(data);
//     } catch (error) {
//         console.log("E: ", error);
//     }
// }

//getAllUsers()

fetch('[https://api.github.com/users/hiteshchoudhary](https://api.github.com/users/hiteshchoudhary)')
.then((response) => {
    return response.json()
})
.then((data) => {
    console.log(data);
})
.catch((error) => console.log(error))

// promise.all
// yes this is also available, kuch reading aap b kro.