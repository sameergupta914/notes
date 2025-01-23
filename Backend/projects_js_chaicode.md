# projects

- color changer project 1->
    
    const buttons = document.querySelectorAll('.button');
    const body = document.querySelector('body');
    
    buttons.forEach(function (button) {
    console.log(button);
    button.addEventListener('click', function (e) {
    console.log(e);
    console.log(e.target);
    if ([e.target.id](http://e.target.id/) === 'grey') {
    body.style.backgroundColor = [e.target.id](http://e.target.id/);
    }
    if ([e.target.id](http://e.target.id/) === 'white') {
    body.style.backgroundColor = [e.target.id](http://e.target.id/);
    }
    if ([e.target.id](http://e.target.id/) === 'blue') {
    body.style.backgroundColor = [e.target.id](http://e.target.id/);
    }
    if ([e.target.id](http://e.target.id/) === 'yellow') {
    body.style.backgroundColor = [e.target.id](http://e.target.id/);
    }
    
    ```
    });
    
    ```
    
    });
    

==bmi calculator project 2->==

const form = document.querySelector('form');
// this usecase will give you empty
// const height = parseInt(document.querySelector('#height').value)

form.addEventListener('submit', function (e) {
e.preventDefault();

const height = parseInt(document.querySelector('#height').value);
const weight = parseInt(document.querySelector('#weight').value);
const results = document.querySelector('#results');

if (height === '' || height < 0 || isNaN(height)) {
results.innerHTML = `Please give a valid height ${height}`;
} else if (weight === '' || weight < 0 || isNaN(weight)) {
results.innerHTML = `Please give a valid weight ${weight}`;
} else {
const bmi = (weight / ((height * height) / 10000)).toFixed(2);
//show the result
results.innerHTML = `<span>${bmi}</span>`;
}
});

==local clock project 3->==
const clock = document.getElementById('clock');
// const clock = document.querySelector('#clock')

setInterval(function () {
let date = new Date();
// console.log(date.toLocaleTimeString());
clock.innerHTML = date.toLocaleTimeString();
}, 1000);

==guess the number project 4, used method of binary sort->==

let randomNumber = parseInt(Math.random() * 100 + 1);

const submit = document.querySelector('#subt');
const userInput = document.querySelector('#guessField');
const guessSlot = document.querySelector('.guesses');
const remaining = document.querySelector('.lastResult');
const lowOrHi = document.querySelector('.lowOrHi');
const startOver = document.querySelector('.resultParas');

const p = document.createElement('p');

let prevGuess = [];
let numGuess = 1;

let playGame = true;

if (playGame) {
submit.addEventListener('click', function (e) {
e.preventDefault();
const guess = parseInt(userInput.value);
console.log(guess);
validateGuess(guess);
});
}

function validateGuess(guess) {
if (isNaN(guess)) {
alert('PLease enter a valid number');
} else if (guess < 1) {
alert('PLease enter a number more than 1');
} else if (guess > 100) {
alert('PLease enter a  number less than 100');
} else {
prevGuess.push(guess);
if (numGuess === 11) {
displayGuess(guess);
displayMessage(`Game Over. Random number was ${randomNumber}`);
endGame();
} else {
displayGuess(guess);
checkGuess(guess);
}
}
}

function checkGuess(guess) {
if (guess === randomNumber) {
displayMessage(`You guessed it right`);
endGame();
} else if (guess < randomNumber) {
displayMessage(`Number is TOOO low`);
} else if (guess > randomNumber) {
displayMessage(`Number is TOOO High`);
}
}

function displayGuess(guess) {
userInput.value = '';
guessSlot.innerHTML += `${guess},` ;
numGuess++;
remaining.innerHTML = `${11 - numGuess}` ;
}

function displayMessage(message) {
lowOrHi.innerHTML = `<h2>${message}</h2>`;
}

function endGame() {
userInput.value = '';
userInput.setAttribute('disabled', '');
p.classList.add('button');
p.innerHTML = `<h2 id="newGame">Start new Game</h2>`;
startOver.appendChild(p);
playGame = false;
newGame();
}

function newGame() {
const newGameButton = document.querySelector('#newGame');
newGameButton.addEventListener('click', function (e) {
randomNumber = parseInt(Math.random() * 100 + 1);
prevGuess = [];
numGuess = 1;
guessSlot.innerHTML = '';
remaining.innerHTML = `${11 - numGuess}` ;
userInput.removeAttribute('disabled');
startOver.removeChild(p);

```
playGame = true;

```

});
}

# Events

document.getElementById('owl').onclick = function(){
alert("owl clicked")
}

attachEvent()
jQuery - on

// type, timestamp, defaultPrevented
//target, toElement, srcElement, currentTarget,
// clientX, clientY, screenX, screenY
// altkey, ctrlkey, shiftkey, keyCode

document.getElementById('images').addEventListener('click', function(e){
console.log("clicked inside the ul");
}, false)

document.getElementById('owl').addEventListener('click', function(e){
console.log("owl clicked");
e.stopPropagation()
}, false)

document.getElementById('google').addEventListener('click',function(e){
e.preventDefault();
e.stopPropagation()
console.log("google clicked");
}, false)

document.querySelector('#images').addEventListener('click', function(e){
console.log(e.target.tagName);
if (e.target.tagName === 'IMG') {
console.log([e.target.id](http://e.target.id/));
let removeIt = e.target.parentNode
removeIt.remove()
}
})

removeIt.parentNode.removeChild(removeIt)

# classes and oops

- ==objects->==
function multipleBy5(num){
    
    ```
          return num*5
      }
    
      multipleBy5.power = 2
    
      console.log(multipleBy5(5));
      console.log(multipleBy5.power);
      console.log(multipleBy5.prototype);
    
      function createUser(username, score){
          this.username = username
          this.score = score
      }
    
      createUser.prototype.increment = function(){
          this.score++
      }
      createUser.prototype.printMe = function(){
          console.log(`price is ${this.score}`);
      }
    
      const chai = new createUser("chai", 25)
      const tea = createUser("tea", 250)
    
      chai.printMe()
    
      /*
    
      Here's what happens behind the scenes when the new keyword is used:
    
      A new object is created: The new keyword initiates the creation of a new JavaScript object.
    
      A prototype is linked: The newly created object gets linked to the prototype property of the constructor function. This means that it has access to properties and methods defined on the constructor's prototype.
    
      The constructor is called: The constructor function is called with the specified arguments and this is bound to the newly created object. If no explicit return value is specified from the constructor, JavaScript assumes this, the newly created object, to be the intended return value.
    
      The new object is returned: After the constructor function has been called, if it doesn't return a non-primitive value (object, array, function, etc.), the newly created object is returned.
    
      */
    
    ```