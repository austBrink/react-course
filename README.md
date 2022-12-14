# CLASS NOTES
## General
You should trim your user strings.

Don't forget you can dynamically style things with the style attribute. While it's nice to have a separation of concerns for styles by putting them in .css, sometimes styles need to be dynamic. Look at this cool example where we are creating a chart...
```
fillHeight = Math.round((props.value / props.maxVal)*100) + '%';
return(
<div style = {{height: fillHeight}}></div>
```
## A Word on Previous State 
When updating state based on values from a previous state, like adding an item to an array, one must use a callback function... 
```
setAState((prevAState) => {
	return [aState, ...prevAState];
});
```
### But why? 
The answer is because react schedules state. In order to ensure that you are indeed using the previous state snapshot, and not one that has been updated by another state that has been asyncronously executed first despite your intent.
## On Styling
### A dumb note.
**DON'T FORGET** about media queries. @media(min-width: 560px) { super special styles }
Remember one can say.... `className = {'name name1'}` and the selector in css looks like `.name.name1`
### Inline Verses Dynamic Class Names
This was a question I posted. I think The answer I have is go with dynamic css classes unless you can't.
Hello all! I have a question on dynamic styling. As I've been doing some projects, I've run across folks dynamically styling form components with invalid values by conditionally assigning classnames.... Something like this....
```
 <div className="form-row">
                    <label htmlFor="contractorNumber">Contractor Number</label>
                    <input
                    type="contractorNumber"
                    name="contractorNumber"
                    id="contractorNumber"
                    value={formValues.contractorNumber}
                    onChange={handleChange}
                    className={formErrors.contractorNumber && "input-error"}
                    />
                    {formErrors.contractorNumber && <span className="error">{formErrors.contractorNumber}</span>}
```
The .css imported took care of setting border to red for all 'error' class names....
In this course I am seeing a general lean toward any dynamic styling getting done with the style attribute. So for this example it could look like this....
```
 <div className="form-row">
                    <label htmlFor="contractorNumber">Contractor Number</label>
                    <input
                    type="contractorNumber"
                    name="contractorNumber"
                    id="contractorNumber"
                    value={formValues.contractorNumber}
                    onChange={handleChange}
                    style = {{border: 'solid 1px red'}}
                    />
                    {formErrors.contractorNumber && <span style = {{color: 'red'}}>{formErrors.contractorNumber}</span>}
```
This is just an example. **Really the question is, to use or not to use inline .css** It's a question that often costs me time when I am writing my jsx and style sheets. Is one faster?
### CSS Modules
#### Styled Components
Haven't seen this before. Tagged template literal. `hello.method```
In CSS the colon denotes a pseudo selector. :hover. AND we can do `&:hover`
## Router 
This note comes from the react book project.... consider I have an array of posts defined in my component. I also am defining my routes here. 
Check out these dynamic routes....
```
<Route
    path = '/post:postSlug'
    element = {(props) => {
         const post = posts.find(
             (post) => post.slug === props.match.params.postSlug
         );
         return <Post post = {post} />;
     }}
/>
```
Here's what happens. We'll link to something like /post/hello. _the hello is the slug_
This route will make this our element... The post that it finds in posts that matches the postSlug. how? The element gets a free props.match and that pertains to... you guessed it. The params in the route. 
## A word on forms
Turns out that the `onSubmit` in a form element comes with a browser default that refreshes the page due to a request to the page host. **That's why** we use `event.preventDefault();`!  
## A word on the optional chaining operator 
There's a super cool question mark in js. `this?.aProperty` The `?.` is the chaining operator, at it checks to make sure what preceeds it is defined. Let's say I wanted a nested portion of an object. 
```
const nestedItem = obj.firstThing?.secondThing;
```
I didn't know this but another easy of doing this is with `&&` so 
```
const nestedItem = obj.firstThing && obj.firstThing.secondThing
```
[see docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) 
## A word on state 
Never use derived state. A state that depends on another state is is continuously updated by another state value or that shares its values. Keep concerns separate.
## A word on hooks
### useStorageState() 
### Use Ref ...
Correct me if I'm wrong but it looks like we can do this ...
```
helloDiv = useRef();
return <div ref = {helloDiv}>Hello</div>
```
### Use Location ...
Given that a component was linked either with <Link> or with useNaviagte hook and we passed and object along witht he url...
`<Link to = '/path' state = {{ key: value }} > ...`
We can later (in that component) extract that state.... 
```
const location = useLocation()
const dataIWant = location?.state?.property || null;
```
As I have found the short circuit operator here `|| null` is very helpful if I want to see some state. LEts suppose all my styles and even api calls depend on if an Id was carried along in state. 
I could then.... 
```
const [ propertyState, setPropertyState ] = useState(dataIWant);

```
## Use Storage State 
This looks really good for future projects, although I'd like to write my own....
```
const [value, setValue] = useStorageState(<storageToUse>, <storageValName>, <initValue>);

```

## A word on Context
__Context__ what is it? 
Context is a means to share state program wide, so that's child to sibling component, or child to parent, or parent to child. It avoids prop drilling. 
So let's say my program needed information on its user (like their username, and if their logged in). 

### Create a speficic 'instance' of context
in a separate file like UserContext, You're gonna need to `import { createContext } from "react"`
(The advantage to a separate file is that you wont need to wrap your context consumers in contect componen).  
It might then be a god idea to initialize the object you want to be in context, like in state. 
```
const init = {user: '', loggedIn: ''};
```
And yeah, lets add that to Context... 
```
const UserContext = createContext(init);
```
One more ting. Don't forget to export this. . . `export default UserContext`

**Cool, you just created some context for a user.**

### Now it would be a good idea to add this to our app. Thats what will do the sharing.... 
So wrap all the component you want to have access with your UserContext like this 
(Dont forget to import your UserContext from UserContext.js first...)
```
<UserContext.Provider value = {{ user }}>
.... other components...
</UserContext.Provider>
```
Okay, so maybe the most important part. _how do I get the context data?_ 
In the file that needs it, you're gonna import it. 
```
import React, {useContext} from "react";
import UserContext from '../context/UserContext;
```
And then... not createContext like we used in the UserContext file. We use useContext... 
```
const {user} = useContext(UserContext);
```
That's about the size of it. Let's recap. 
1. You need state to be accessible between siblings and from parent to child but you're too lazy to prop drill...
2. You'll need to create a context file. This should import CreateContext. and export a CreateContext(initValues)
3. You'll need to import this file into the component that _has_ the item to be shared. This is your context provider.
 Great, now you'll wrap the components that need access to this var with <YourContextYouExported.Provider value = {stuffNeeded}> ...
4. Now you need to grab your context. In this file you need it, import useContext from react along with your context file. 
5. Deconstruct the data you need from context value.... `const {stuffNeededByName} = useContext(MyContextFile);`     
