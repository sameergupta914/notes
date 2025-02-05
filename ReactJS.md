# ReactJS

- React is a javascript library. it is all about component
- Its easy to create UI in reactjs as compare to simple javascript.
- It follows declarative approach which means you have to provide only the end state and rest react manages itself.
- It uses SPA approach, meaning it is based on single page application apporach

- To create react app->
    `npx create-react-app <appname>`
   ` cd <appname>`
   ` npm start`

- To create a component->
    - create a js file and css file, keep first letter of word capital in the name of both files and link css into js file. eg:
        `ItemDate.js`, `ItemDate.css` , to link css into js-> `import './ItemDate.css';`
    - in js file, create a function which is called component, and return a javascript xml file. and export the component. 
    - where we have to use that component, import it.

- `{props.children}` -> to make visible inside of tag
- props ->

- onClick -> this hook take function as input. eg- `<button className='btn' onClick={clickHandler}>` , `function clickHandler(){ console.log('trying onclick hook'); }`

- Parent contacting child->
    - `useState(<initializingvalue>)` ->this hook returns array containing 2 value, value of variable and a function which returns the changed value. eg-> `let [name, setName]=useState(props.name)` , `setName('gupta');`

    - to use useState for multiple, `const [fullProductInput, setfullProductInput]= useState({ title:'', date:'' });` , `function setfullProductInput(){  }`

- Child contacting parent->
    - create a function and pass it to the child and child will return the obj as argument. eg->
        in app.js
        ```javascript
         function tryingchild(data){
            console.log('inside app.js');
            console.log(data);
         }
         <NewProduct contacting={tryingchild} />
        ```
        in NewProduct.js
        ```javascript
        function grandchild(data){
             console.log('inside newproduct.js');
             props.contacting(data);
        }
        <ProductForm finall={grandchild} />
        ```
        in ProductForm.js
        ```javascript
        const data={
            name:newName,
            date:newDate
        } 
        props.finall(data);
        ```

- `event.target.value` -> gives the value from event occuring
- `event.preventDefault()` -> prevents default behaviour
- `<input type='text' value={newName}>` -> value tag links the newName to the value of input.
- 
