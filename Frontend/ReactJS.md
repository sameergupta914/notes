# ReactJS

- React is a javascript library. it is all about component
- Its easy to create UI in reactjs as compare to simple javascript.
- It follows declarative approach which means you have to provide only the end state and rest react manages itself.
- It uses SPA approach, meaning it is based on single page application apporach

- To create react app->
    `npx create-react-app <appname>`
   ` cd <appname>`
   ` npm start`
- if a error come related to code not found use `npm install --save-dev ajv@^7`

- To create a component->
    - create a js file and css file, keep first letter of word capital in the name of both files and link css into js file. eg:
        `ItemDate.js`, `ItemDate.css` , to link css into js-> `import './ItemDate.css';`
    - in js file, create a function which is called component, and return a javascript xml file. and export the component. 
    - where we have to use that component, import it.

- `{props.children}` -> to make visible inside of tag
- props ->

- onClick -> this hook take function as input. eg: `<button className='btn' onClick={clickHandler}>` , `function clickHandler(){ console.log('trying onclick hook'); }`
- onChange -> comes in action as soon as there is any change in the input.

# Prop Drilling (Parent contacting child), useState hook
- useState hook -> `useState(<initializingvalue>)` ->this hook returns array containing 2 value, value of variable and a function which returns the changed value. eg: `let [name, setName]=useState(props.name)` , `setName('gupta');`

- to use useState for multiple, `const [fullProductInput, setfullProductInput]= useState({ title:'', date:'' });` , `function setfullProductInput(){  }`

# State Lifting (Child contacting parent)->
- create a function and pass it to the child and child will return the obj as argument. eg:
    - in app.js
     ```javascript
         function tryingchild(data){
            console.log('inside app.js');
            console.log(data);
         }
         <NewProduct contacting={tryingchild} />
        ```
    - in NewProduct.js
        ```javascript
        function grandchild(data){
             console.log('inside newproduct.js');
             props.contacting(data);
        }
        <ProductForm finall={grandchild} />
        ```
    - in ProductForm.js
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
- Whenever using map to list elements always use key tag. eg: `key={data.id}` 

# useEffect hook 
- it manages side effects like making an api call, change in the document title, modifying browsers history etc. [agar aap koi task karwana chahte ho component ke render hone ke baad, uss task ko hm iss hook ke andr define karte hain aur usko hi side effects kehte hain]. contains 2 parts, callback function and array of dependencies.
    - var1(every render): it executes after every rendering. eg: `useEffect(()=>{ console.log( 'UI rendering done' )});`
    - var2(first render): only after first rendering. eg: `useEffect(()=>{ console.log( 'UI rendering done' )}, []);`
    - var3(whenever dependency changes + first render): eg: `useEffect(()=>{ console.log( 'UI rendering done' )}, [text]);`
    - var4(first remove listener then add listener): eg: `useEffect(()=>{ console.log('listener added') return ()=>{ console.log('listener removed) }}, [text]);`

# React Toast: 
- `npm i react-toastify` ->to install
- in index.js file:
-  `import { ToastContainer } from 'react-toastify';`
-  `import "react-toastify/dist/ReactToastify.css";`
    ```javascript
    <div>
    <App />
    <ToastContainer/>
    </div>
    ```
- Where you have to use: `import { toast } from "react-toastify";`, 
- use like these:  
        `toast.warning('Like Removed');`
        `toast.success('Liked Successfully');`
        `toast.error('something went wrong');`

- `npm i react-hot-toast`
- in index.js file: `import { Toaster } from 'react-hot-toast';`, ` <App /> <Toaster/>`
- where you have to use: - `import toast from "react-hot-toast";`



- To fetch an API call,eg:  `import { apiUrl } from './data';`
                            `let res= await fetch(apiUrl);`
                            `let output=await res.json();`

- In map function if you use {} then you have to return and if use () then without return. eg:
    ```javascript
    getCourses().map((course)=>(
                 <Card key={course.id}/>
            ))
    ```
- If you use useState hook with an API call then if passing `null` as intial value in useState hook then it can show error while fetching so to avoid that use spinner logic or else pass `[]` instead of null. eg: `const [courses, setCourses]=useState([]);`

- Controlled components: `value='formData.firstName'`, eg:
    <input type='text' placeholder='first name' onChange={changeHandler} name='firstName' value={formData.firstName}/>

# Form
- to use useState for multiple variables:
        ``` 
        const [formData, setFormData]=useState({
            firstName:'', 
            lastName:'',
            commentbox:false,
            candidates:false,
            offers:false,
            samemode:'',
            country:''
        });
        
        function changeHandler(event){
            const {name, value, checked, type}= event.target
            setFormData(prevData=>{
                return {
                ...prevData,
                [name]: type==='checkbox' ? checked : value
                }
            });
        } ```

- On clicking submit button an automatic onSubmit hooks occurs.

- Checkbox:  on using htmlFor tag it should match with name tag & id tag. eg: ```
            <input type='checkbox' onChange={changeHandler} name='commentbox' id='commentbox' checked={formData.commentbox}/>
            <label htmlFor='commentbox'>Comments </label>  ```

- Radio: to select either one option they all have same name tag. eg:
    ```
            <input type='radio' onChange={changeHandler} name='samemode' value='everything' id='everything' checked={formData.samemode==='everything'}/>
            <label htmlFor='everything'>Everything</label>

            <input type='radio' onChange={changeHandler} name='samemode' value='email' id='email' checked={formData.samemode==='email'}/>
            <label htmlFor='email'>Same as Email</label> ```

# React Router

- `npm i react-router-dom` -> to install router
    - `import { BrowserRouter } from 'react-router-dom';` 
    - `<BrowserRouter> <App /> </BrowserRouter>` -> in index.js

- navigate between multiple pages without refreshing the page.

- `<Routes></Routes>` -> used to create multiple Routes
- `import { Route, Routes } from 'react-router-dom';` -> to import.
- `<Route path='/' element={<component/>}></Route>` -> single route
    - `<Outlet/>` -> allows appear of its children
    - `<Route index element={<component/>}` -> as a index
    - `<Route path='*' element={<NotFound/>}/>` ->if anyother besides given routes

- useNavigate hook: to navigate from one page to anyother
    - `import { useNavigate } from "react-router-dom";` 
        `const navi=useNavigate();`
    - `navi('/');` -> to navigate to '/'.
    - `navi(-1);` -> to navigate to previous page.
- navigate hook: `<navigate to='/'>`

- Link: `<Link to='#'> <p className="forgot">Forgot Password</p> </Link>` -> # stay on same page.
- NavLink: same as link but add `classname='active'` in the link.

- extra: `import { AiOutlineEye, AiOutlineEyeInvisible } from "react-icons/ai";`

# Context API

- step 1: `export const AppContext= createContext();`
- step 2: `export default function AppContextProvider({children}) { return <AppContext.Provider value={value}> {children} </AppContext.Provider>; }`
- step 3: `const value ={ };`
- in index.js: `<AppContextProvider> </App.js> </AppContextProvider>`
- to consume: `const { }= useContext(AppContext);`

# useLocation hook:
-  The useLocation hook in React Router is used to access the current URL location object. This is useful for getting the pathname, search parameters, or state passed between pages.
- The location object contains:
    - pathname → The current URL path (/home, /dashboard, etc.)
    - search → Query parameters (?id=123&name=John)
    - hash → The URL fragment (#section1)
    - state → Data passed via useNavigate or <Link>
- syntax:
    `import { useLocation } from 'react-router-dom';`
    ` const location=useLocation();  `
- use:
     ` if(location.pathname.includes('tags')){ const tag= location.pathname.split('/').at(-1).replaceAll('-', ' ');  ` 
- eg:
    ```
     import { useLocation } from "react-router-dom";
        function LocationExample() {
        const location = useLocation(); 
        return (
          <div>
            <p>Pathname: {location.pathname}</p>
            <p>Search: {location.search}</p>
            <p>Hash: {location.hash}</p>
            </div>
        );
        }```

- If the URL is http://localhost:3000/profile?id=42#section1,
- Output:
        Pathname: /profile
        Search: ?id=42
        Hash: #section1


# useSearchParams hook: 
- The useSearchParams hook is used to read and modify query parameters in the URL without affecting the component's state.
- useSearchParams returns:
     - searchParams → An instance of URLSearchParams to read query parameters.
     - setSearchParams → A function to update query parameters.
- syntax:
     ` import { Route, Routes, useLocation, useSearchParams } from 'react-router-dom';`
     `const [searchParams, setSearchParams]= useSearchParams();`
- use:
    ` const page=searchParams.get('page') ?? 1;`
- eg:
    ```
    import { useSearchParams } from "react-router-dom";
        function SearchParamsExample() {
        const [searchParams, setSearchParams] = useSearchParams();
        return (
            <div>
            <p>Query ID: {searchParams.get("id")}</p>
            <button onClick={() => setSearchParams({ id: "123" })}>
                Change Query ID
            </button>
            </div>
        );
        }
        ```

- If the URL is http://localhost:3000/profile?id=42,
- Output: Query ID: 42

# Redux

- `npm install @reduxjs/toolkit react-redux` -> to install.

- Slice template eg:
    ```
    import { createSlice } from '@reduxjs/toolkit'

    export const CartSlice = createSlice({
    name: 'cart',
    initialState:[],
    reducers: {
        add: (state, action) => {
        state.push(action.payload);
        },
        remove: (state, action) => {
        return state.filter((item)=>item.id !== action.payload);
        },
    },
    })

    export const { add, remove} = CartSlice.actions
    export default CartSlice.reducer
    ```
    - action.payload: action.payload jo input me send kr rhe hain usko show krta hain
    - State: state current conditon to show kr rha hain

- Store: The store is a central place where the entire state of your application lives. It holds the data and serves as the single source of truth for your app's state. store template:
    ```
    import { configureStore } from '@reduxjs/toolkit'
    import  CartSlice  from './slices/CartSlice'
    // import CartReducer from './slices/CartSlice';

    export const store = configureStore({
    reducer: {
        cart: CartSlice,

        },
    })   ```

- Dispatch: Dispatch is a function that allows you to send actions to the Redux store to trigger state changes. When an action is dispatched, the reducers evaluate the action and return a new state.

- useDispatch: The primary purpose of useDispatch is to dispatch actions to the Redux store.It gives you access to the dispatch function, allowing you to trigger changes to the state by sending actions to the store. `const dispatch=useDispatch();`. 
- eg:
    ```
    const dispatch=useDispatch();

    const addToCart=()=>{
        dispatch(add(post));
        toast.success('Item added to Cart');
    }
    const removeFromCart=()=>{
        dispatch(remove(post.id));
        toast.error('Item removed from Cart');
    }  ```

- useSelector : The primary purpose of useSelector is to read data from the Redux store. It allows your component to access and subscribe to specific pieces of the Redux state. When the state your component depends on changes, useSelector will trigger a re-render. 
    - syntax template:
    `import { useSelector } from "react-redux"`,  `const {cart}=useSelector((state)=>state);`