# Javascript notes

# basics

- console.log(x) , console.log(x), console.log(x) can also be written as
console.table([ x,y,z ])
- use const and let only and avoid var
- to initialize large number we use bigint
- symbol is a datatype use for uniqueness
- typeof null is object and typeof undefined is undefined
- conversion to number-
"33" - 33
"sameer" - NaN
true - 1
null - 0
eg- let str="33"; let converttonum=Number(str); let convertback=String(converttonum)
- conversion to bool
1 - true
0 - false
"" - false
"xyz" - true
- confusing conversions-
console.log("02" > 1) ->true
console.log(null > 0) ->false
console.log(null >= 0) ->true
console.log(null == 0) -> false
console.log("2" === 2) ->false
- primitive data types- string, number, Boolean, null, undefined, symbol, bigint
- reference or non-primitive datatypes- array, objects, functions
- to protect from getting polluted from global scope we can declare function in this way or IMMEDIATELY INVOKED FUNCTION EXPRESSIONS(IIFE) ->
(function chai(){
console.log(`im on fire`)
})(); ->im on fire
or
( (name)=>{
console.log(`hehehe` ${name})
})(`sameer`); ->hehehe sameer
- falsy values -> false, 0, -0, 0n, "", null, undefined, NaN
- truthy values-> true, "0", 'false', " ", [], {}, function(){},
- 

# strings

- str.length- to find length
- to find index of some target- str.indexOf(target)
- to find last apperance index-str.lastIndexOf(targetx2)
- str.slice(start,end) or "xyz".slice(start,end) - prints index from start to end, not including end. it is similar to substr function but substr takes starting point and length as variables.
- str.replace("word to replace", "word to replace with") eg- const str="hello world"; console.log(str.replace("he","javascript"))
- to split the string into array- const words=value.split(" [this is delimiter] "); console.log(words); if we add space then is divides on basis of space if comma then comma, if b then b.
- to trim out extra spaces from the begining and end use- str.trim( )
- str.toUpperCase() and str.toLowerCase() to capital and small
- to convert string to integer - parseInt("digit") & parseFloat("") for float
- instead of normally concatenating strings we can be this--
let abc="sameer"; let xyz="30"
console.log("hello i am ${abc} and my balance is ${xyz} million)
instead of console.log( "i am" + abc + "dbjashbja")
- console.log(xyz.indexOf('a')) ->1
- console.log(xyz.charAt(2)) ->m
- let xyzz=xyz.substring(0,4) ->same
- let xyzz=xyz.slice(-7,4) ->?
- xyz.trim() -> remove all the spaces present
- let xyzz="sammer is king";
console.log(xyzz.replace('is','was'))
- console.log(xyzz.includes('king')) ->true
- console.log(xyzz.split(' ')) ->gives an array of string conssisting of string present in this but dividedby space( ' ' )

# arrays

- arr.push(x)- to push in array, arr.pop( )- to pop from last, arr.shift()- to pop from front, arr.unshift(x)- to push from front
- lenght-> arr.length
- finalarray=arr.concat(secarray); console.log(finalarray) - to concatenate both arrays
- arr.forEach(function)- this call this function for size of array times. similar to forloop, but this expect a function as its argument
- to add in the front of array..its a type of shifting the array to the right- arr.unshift(digit)
to remove from the front and all remaining elements gets shiffted to left- arr.shift()
- arr.includes(9) ->tells whether 9 is present in the array or not
- arr.indexOf(3) ->tells the index of 3
- arr.join() ->converts the array to strings seperated by commas
- arr.slice(1,n) ->prints the array from index 1 to n-1
- arr.splice(1,4) ->prints the array from index 1 to 4 and orginal array gets cuts... rather having from 1 to n it will now have 5 to n
- to merge two arrarys-
let mix=arr1.concat(arr2) or
let mix=[...arr1, ...arr2, ...arr3 ] and so on...it will merge all arrays togther
- if you have an array inside of an array inside of an array you can flat it out by -> let newarr=arr.flat(infinity) ->this infinity is the depth of arrays
- to convert to array ->console.log(Array.from("sameer")) ->['s', 'a', 'm', 'e', 'e', 'r' ] or
let x=1,y=2,z=3; console.log(Array.of(x,y,z)); ->[1,2,3]

# classes

- constructor- class animal{
constructor(name, legcount,speak){
this.name=name;
this.legcount=legcount;
this.speak=speak;
}
static mytype(){
console.log("aniimall")
}
speak(){
console.lof("hi there" + this.speak )
}
}
let dog=new animal( "doggy" , 4 , "bhowbhow" );
dog.speak();
console.log( animal.mytype( ) )
- create object- new animal...
- function under class, can only be called on the object of the class- speak()
- static methods are associated to class itself not object

# JSON and APIs

- to convert object to string-
const users='{"name": "sameer", "age": 20, "gender":"male"}'
console.log(users["0"]) //print sameer
- to convert back to object-
const user=JSON.parse(users)
console.log(user["gender"]); //print male
- to convert object to string-
const finalstring=JSON.stringify(user)
console.log(finalstring)
- syntax -> {
{ },
{ },
{ }
}
or can be in array form -> [
{ },
{ }
]

# dates

let x= new Date()
console.log(x.toString());  ->todays date
let y=new Date(2024,6,21,5,3)   or("01-14-2023")
y.toString()   ->Mon july 21 2024
y.toLocaleString()   -> Mon july 21 2024 5:03:00 AM
let z= Date.now()  ->current date in miliseconds
let a=new Date();  console.log(a.getMonth()+1)
or a.getDay()

- customize timzezone and etc etc-
a.toLocaleString('Default' , {weekday:"long" , timeZone: 'etc'})

# math and numbers

- Math.random(), Math.ceil(2.3)->3, Math.floor(2.3)->2, Math.round(value), [Math.ax](http://math.ax/)(5,10,15),Math.pow(value,2),
- let a=100; console.log(a.toString()) ->converted to string
- console.log(a.toFixed(2)) ->100.00
- console.log(a.toPrecision(3)) ->i/p->123.89 o/p->124
- let a=4.2; Math.round(4.2) ->4
Math.ceil(4.2) ->5 Math.floor(4.2) ->4
Math.min(4,2,5,9,3) ->2
- to print random numbers bw a min vallue and a max value-
console.log( Math.floor( Math.random() * (max-min +1 ))+10 )
- math.random gives random numbers between 0 and 1 including both

# objects

const mySyn= symbol("key1")

- obj: {
key1 : value1,
key2 : value2,
key3 : value3'
[mySyn] : "mykey1"
}
Object.keys()- prints all the keys
Object.values() - prints all the values
Object.entries()- prints keys with its values
    - to add more properties in an object-
    let newobj = Object.assign( {}, obj, {newproperty: "newvalue" });
    console.log(newobj)
- console.log(obj.key1) or console.log(obj["key1"]) or const {key1 : k}=obj console.log( k ) ->to access key1 value
- to access symbol in object ->obj[mySyn]
- to lock your object or didnt want your object to be modified -> Object.freeze(obj)
- to add function in object ->
obj.greeting=function(){
console.log("hellooo");
}
console.log(obj.greeting());
    - obj.greeting2=function(){
    console.log(`hellooo, ${this.key1}`);
    } ->how to access inner items

OBJECTS SINGLETON

- you can nest object inside object inside object...
const obj1={
fullname: {
firstname:"sameer",
lastname: "gupta"
}
}
console.log(obj1.fullname.firstname); ->sameer
- to merge objects -> const obj3= Object.assign({}, obj1, obj2,obj4) or
const obj3= {...obj1, ...obj2, ...obj3 }
- to check if the given value is present in the object or not ->
obj1.hasOwnProperty('xyz') ->it finds xyz in obj1, if present returns true


# conditionals and loops

- NULLLISH COALESCING OPERATOR ( ?? )->
let a;
a=5 ?? 10 ->5
a=null ?? 10 ->10
a=undefined ?? 10 ->10
a=null ?? 10 ?? 5 ->10
- TERNIARY OPERATOR ( ? ) ->
condition ? true : false
eg->
let a=100;
a<=80 ? console.log("ok") : console.log("not ok")
- SWITCH(x){
case 1:
case2:
.
.
.
default():
}
- FOR-OF LOOP
can be used for arrays, maps but not objects
for(const i of arr){
console.log(i)
}
- FOR-IN LOOP
can be used in objects, arrays
syntax with example of obj
for(const key in obj1){
console.log(key, obj1[key])
}
syntax for array
for(const key in arr){
console.log(arr[key])
}
- FOR-EACH LOOP
-> array_name.forEach( function(val){
console.log(val);
} )
-> arr.forEach( (item,index,arr)=>{
console.log(item,index,arr);
})
->const mycode=[ {lang : "js", full : "javascr"}, {lang: "", full : ""}, {} ]
mycode.forEach((item)=>{console.log(item.lang);
}) ->this shows how to acccs each object in an array
- 

# map and filter

- const map= new Map()
map.set( 'in', "india" )
    
    for(const key of map){
    console.log(key)
    }    or
    for(const [key, value] of map){
    console.log(key,value)
    }
    
- FILTER
const arr= [1,2,3,4,5,6]
const newnums=arr.filter( (num)=>num>4 )
console.log(newnums); -> [5,6]
//agar {num>4} kar rhe ho iske jageh to sath me return use krna prega i.e. {return num>4}
- MAP
let x=arr.map((num)=>{return num+10})
console.log(x); ->[11,12,13,14,15,16]
- chaining->
using multiple filter or maps together
let x= arr.map(()=>{}).map(()=>{}).filter(()=>{})
- REDUCE
let y=arr.reduce(function(acc, currval){
console.log(acc, currval) }, 0)
console.log(y) ->21
    
    let y=arr.reduce((acc,currval)=>acc+currval, 0)
    console.log(y)  ->21
