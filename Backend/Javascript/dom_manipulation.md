# DOM

- to get something from the html document, for eg we want to get all the links from docs -> `console.log(document.link);`

- to access something by their id -> `document.getElementById(' ')`,
- by class_name-> `x.getElementByClassName(' ')`

- to manipulate something with their id -> `document.getElementById('').innerHTML`
- `x.getElementById('id_name').getAttribute('attribute')` -> attribute can be id or class or etc =>o/p is attribute_name

- setAttribute to change any attribute-> `x.setAttribute('attribute', 'attribute_name')`

- `id_name.innerText` , `id_name.innerContent` , `id_name.innerHTML` -> innertext gives the visible text and innercontent gives thewhole texts, visible and not visible both.

# querySelector

`x.querySelector('h2')`
`x.querySelector(#id_name)`
`x.querySelector(.class_name)`

('input[type="password"]')
('p : first-child')
`x.querySelector('ul')`

- `const myul=x.querySelector('ul'); myul.querySelector('li')`

`const turng=myul.querySelector('li'); turng.syle.backgroundColor="green"`

x.querySelectorAll('li') ->array jaisa dikhta hai
- nodelist and HTML collection are similar to array not not exactly same.
    - converting HTML collection to array->
    x.getElementsByClassName('list-item')
    const y=x.getElementsByClassName('list-item')
    Array.from(y)
- to access first and last child ->
parent.firstElementChild
parent.lastElementChild
- to access parent element-> x.parentElement
- x.nextElementSibling
- CREATE ELEMENT IN DOM
const div=document.createElement('div')
console.log(div)
div.className="main"
div.id="cum"
div.setAttribute("title", "generated title")
div.style.backgroundColor="green"
div.style.padding="12px"
// div.innerText="sameer is king"
const addtext=document.createTextNode("sameer is king")
div.appendChild(addtext)
document.body.appendChild(div)
- EDIT AND REMOVE ELEMENT IN DOM
    
    <ul class="language">
    <li>java</li>
    
    ```
    function addlang(langname){
        const li=document.createElement('li')
        li.innerHTML=`${langname}`
        document.querySelector('.language').appendChild(li)
        }
        addlang("python")
    
    ```
    
    optimisedway->
    function optaddlang(langname){
    const li=document.createElement('li')
    li.appendChild(document.createTextNode(langname))
    document.querySelector('.language').apppendChild(li)
    }
    optaddlang('golang')
    
    EDIT->
    const seclang=document.querySelector("li : nth-child(2)")
    console.log(seclang)
    const newli=document.createElement('li')
    newli.textContent="hehe"
    seclang.replaceWith(newli)
    edit2->
    const firstlang=document.querySelector("li : first-child")
    firstlang.outerHTML='<li>typescript...'
    
    REMOVE->
    const lastlang=document.querySelector("li : last-child")
    lastlang.remove()