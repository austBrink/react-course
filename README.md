## Notes on dynamic styles.
Don't forget you can dynamically style things with the style attribute. While it's nice to have a separation of concerns for styles by putting them in .css, sometimes styles need to be dynamic. Look at this cool example where we are creating a chart...
```
fillHeight = Math.round((props.value / props.maxVal)*100) + '%';
return(
<div style = {{height: fillHeight}}></div>
```
## On Styling
### A dumb note.
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
**DON'T FORGET** about media queries. @media(min-width: 560px) { super special styles }
## General
You should trim your user strings.

