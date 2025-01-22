# functions

- function syntax
function funcname(n){ //no need to mention type of n along with it
...
return xyz;
}
    - declare function-
    let ans=funcname(10)
    console.log(ans);
    - callback function-
    it passes function as argument
    function funcname(a,b,fn){
    }
    or
    function funcname(a, b, function(n){return n*n} ) {
    }
- to pass default value in function ->
function xyz(user = "sam"){
return `${user} just logged in`
}
console.log(xyz("sameer")) ->sameer just logged in
console.log(xyz( )) ->sam just logged in(in case no argument is paased)
- function calculate( ...num1){
return num1 }
console.log(calculate(200,450,560,233))
->[200,450,560,233]
- types of way to define functions->
function addone(num){
return num+1
} or
const addone=function(num){
return num+1
} ->int his case we cant access function before declaration like addone(5) can not be above this function

# arrow function

- this keyword is used inside object and also globally but not inside of function
- example for using this keyword inside object->
const user={
name : "sameer",
age : 21,
welcomm : function(){
console.log(`${this.username},heyyy`);
}
}
user.welcomm() ->sameer heyyy
user.name="sam"
user.welcomm -> same heyyy
- ARROW FUNCTION
    - const addtwo=(num1,num2) =>{
    return num1+num2
    }
    console.log(addtwo(2,4))
    - when we have only one line of code inside arrow function we can write in this way also->
    const addtwo=(num1,num2)=>(num1+num2)
    console.log(addtwo(2,4))

# asynchronous functions

- javascript is single threaded..
- this async functions behave in a type of multi threaded way.. like.. there are some tasks JS thread needs to wait for so it temporarily do some other tasks in the meantime..
- setTimeout and readfile are 2 examples of an asynchronous functions
function xyz(){
console.log("hey babydoll") }
setTimeout(xyz, 1000);
this will call the xyz func after one second.
readfile("a.txt", xyz);
this will execute the xyz func after it reads the file, till then it will execute aage ke codes
- syntax for readFile-
const fs=require("fs");
fs.readFile("a.txt", "utf-8", function(err, data){
console.log(err);
console.log(data);
data=data+" its me."; fs.writeFile;
});
- callback hell ex- calling settimeout inside settimeout.
- if remaining code depends on readfile then those codes should be inside the async functions.
