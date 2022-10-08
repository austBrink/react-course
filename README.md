# Things of noteworthiness from maximillian react course...  
## A Word on Previous State 
When updating state based on values from a previous state, like adding an item to an array, one must use a callback function... 
```
setAState((prevAState) => {
	return [aState, ...prevAState];
});
```
### But why? 
The answer is because react schedules state. In order to ensure that you are indeed using the previous state snapshot, and not one that has been updated by another state that has been asyncronously executed first despite your intent.

## A word on forms
Turns out that the `onSubmit` in a form element comes with a browser default that refreshes the page due to a request to the page host. **That's why** we use `event.preventDefault();`! 
