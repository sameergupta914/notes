- prototype
// let myName = "hitesh "
// let mychannel = "chai "
    
    // console.log(myName.trueLength);
    
    let myHeros = ["thor", "spiderman"]
    
    let heroPower = {
    thor: "hammer",
    spiderman: "sling",
    
    ```
      getSpiderPower: function(){
          console.log(`Spidy power is ${this.spiderman}`);
      }
    
    ```
    
    }
    
    Object.prototype.hitesh = function(){
    console.log(`hitesh is present in all objects`);
    }
    
    Array.prototype.heyHitesh = function(){
    console.log(`Hitesh says hello`);
    }
    
    // heroPower.hitesh()
    // myHeros.hitesh()
    // myHeros.heyHitesh()
    // heroPower.heyHitesh()
    
    // inheritance
    
    const User = {
    name: "chai",
    email: "[chai@google.com](mailto:chai@google.com)"
    }
    
    const Teacher = {
    makeVideo: true
    }
    
    const TeachingSupport = {
    isAvailable: false
    }
    
    const TASupport = {
    makeAssignment: 'JS assignment',
    fullTime: true,
    **proto**: TeachingSupport
    }
    
    Teacher.**proto** = User
    
    // modern syntax
    Object.setPrototypeOf(TeachingSupport, Teacher)
    
    let anotherUsername = "ChaiAurCode     "
    
    String.prototype.trueLength = function(){
    console.log(`${this}`);
    console.log(`True length is: ${this.trim().length}`);
    }
    
    anotherUsername.trueLength()
    "hitesh".trueLength()
    "iceTea".trueLength()
    
- ==bind->==
    
    class React {
    constructor(){
    this.library = "React"
    this.server = "[https://localhost:300](https://localhost:300/)"
    
    ```
          //requirement
          document
              .querySelector('button')
              .addEventListener('click', this.handleClick.bind(this))
    
      }
      handleClick(){
          console.log("button clicked");
          console.log(this.server);
      }
    
    ```
    
    }
    
    const app = new React()
    
- ==call->==
    
    class React {
    constructor(){
    this.library = "React"
    this.server = "[https://localhost:300](https://localhost:300/)"
    
    ```
          //requirement
          document
              .querySelector('button')
              .addEventListener('click', this.handleClick.bind(this))
    
      }
      handleClick(){
          console.log("button clicked");
          console.log(this.server);
      }
    
    ```
    
    }
    
    const app = new React()
    
- ==getter and setter==
class User {
constructor(email, password){
this.email = email;
this.password = password
}
    
    ```
      get email(){
          return this._email.toUpperCase()
      }
      set email(value){
          this._email = value
      }
    
      get password(){
          return `${this._password}hitesh`
      }
    
      set password(value){
          this._password = value
      }
    
    ```
    
    }
    
    const hitesh = new User("[h@hitesh.ai](mailto:h@hitesh.ai)", "abc")
    console.log(hitesh.email);
    
- ==inheritance==
class User {
constructor(username){
this.username = username
}
    
    ```
          logMe(){
              console.log(`USERNAME is ${this.username}`);
          }
      }
    
      class Teacher extends User{
          constructor(username, email, password){
              super(username)
              this.email = email
              this.password = password
          }
    
          addCourse(){
              console.log(`A new course was added by ${this.username}`);
          }
      }
    
      const chai = new Teacher("chai", "chai@teacher.com", "123")
    
      chai.logMe()
      const masalaChai = new User("masalaChai")
    
      masalaChai.logMe()
    
      console.log(chai instanceof User);
    
    ```
    
- ==mathPI==
const descripter = Object.getOwnPropertyDescriptor(Math, "PI")
    
    ```
      // console.log(descripter);
    
      // console.log(Math.PI);
      // Math.PI = 5
      // console.log(Math.PI);
    
      const chai = {
          name: 'ginger chai',
          price: 250,
          isAvailable: true,
    
          orderChai: function(){
              console.log("chai nhi bni");
          }
      }
    
      console.log(Object.getOwnPropertyDescriptor(chai, "name"));
    
      Object.defineProperty(chai, 'name', {
          //writable: false,
          enumerable: true,
    
      })
    
      console.log(Object.getOwnPropertyDescriptor(chai, "name"));
    
      for (let [key, value] of Object.entries(chai)) {
          if (typeof value !== 'function') {
    
              console.log(`${key} : ${value}`);
          }
      }
    
    ```
    
- ==myclasses->==
// ES6
    
    class User {
    constructor(username, email, password){
    this.username = username;
    this.email = email;
    this.password = password
    }
    
    ```
      encryptPassword(){
          return `${this.password}abc`
      }
      changeUsername(){
          return `${this.username.toUpperCase()}`
      }
    
    ```
    
    }
    
    const chai = new User("chai", "[chai@gmail.com](mailto:chai@gmail.com)", "123")
    
    console.log(chai.encryptPassword());
    console.log(chai.changeUsername());
    
    // behind the scene
    
    function User(username, email, password){
    this.username = username;
    this.email = email;
    this.password = password
    }
    
    User.prototype.encryptPassword = function(){
    return `${this.password}abc`
    }
    User.prototype.changeUsername = function(){
    return `${this.username.toUpperCase()}`
    }
    
    const tea = new User("tea", "[tea@gmail.com](mailto:tea@gmail.com)", "123")
    
    console.log(tea.encryptPassword());
    console.log(tea.changeUsername());
    
- ==object getset->==
const User = {
_email: 'h@hc.com',
_password: "abc",
    
    ```
      get email(){
          return this._email.toUpperCase()
      },
    
      set email(value){
          this._email = value
      }
    
    ```
    
    }
    
    const tea = Object.create(User)
    console.log(tea.email);
    
- ==oop->==
const user = {
username: "hitesh",
loginCount: 8,
signedIn: true,
    
    ```
          getUserDetails: function(){
              //console.log("Got user details from database");
              // console.log(`Username: ${this.username}`);
              console.log(this);
          }
    
      }
    
      //console.log(user.username)
      //console.log(user.getUserDetails());
      // console.log(this);
    
      function User(username, loginCount, isLoggedIn){
          this.username = username;
          this.loginCount = loginCount;
          this.isLoggedIn = isLoggedIn
    
          this.greeting = function(){
              console.log(`Welcome ${this.username}`);
    
          }
    
          return this
      }
    
      const userOne = new User("hitesh", 12, true)
      const userTwo = new User("ChaiAurCode", 11, false)
      console.log(userOne.constructor);
      //console.log(userTwo);
    
    ```
    
- ==property get set->==
function User(email, password){
this._email = email;
this._password = password
    
    Object.defineProperty(this, 'email', {
    get: function(){
    return this._email.toUpperCase()
    },
    set: function(value){
    this._email = value
    }
    })
    Object.defineProperty(this, 'password', {
    get: function(){
    return this._password.toUpperCase()
    },
    set: function(value){
    this._password = value
    }
    })
    
    }
    
    const chai = new User("[chai@chai.com](mailto:chai@chai.com)", "chai")
    
    console.log(chai.email);
    
- ==static property->==
class User {
constructor(username){
this.username = username
}
    
    logMe(){
    console.log(`Username: ${this.username}`);
    }
    
    static createId(){
    return `123`
    }
    }
    
    const hitesh = new User("hitesh")
    // console.log(hitesh.createId())
    
    class Teacher extends User {
    constructor(username, email){
    super(username)
    this.email = email
    }
    }
    
    const iphone = new Teacher("iphone", "[i@phone.com](mailto:i@phone.com)")
    console.log(iphone.createId());
    