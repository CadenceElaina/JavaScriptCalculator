# Web Dev Simplified - JavaScriptCalculator
Below are some of the steps taken to make this calculator in my own words - credit to https://www.youtube.com/watch?v=j59qQ7YWLxw&t=1266s - Web Dev Simplified - this was a tutorial led project that I found helpful to implement JS and the functions and a class onto an actual webpage - The format of the notes are certainly not consistent, but helped clarify the process for me. 

HTML
1. Use ! command to create HTML head etc. 
2. link our styles.css sheet and our script.js
3. Create a div for our calculator-grid class
4. Div with our output class
5. inside our output class div we include two inner divs to house our prior operand to our current operand. Can fill in with dummy text to help you see what you are styling in your CSS
6. Create our buttons for the calculator - All clear (AC) class span-two (Spans length of 2 buttons), Delete (DEL), Division, next grid row down, 1, 2, 3, multiply (*),
next row down, 4,5,6, addition (+), next row, 7,8,9, subtraction (-), next row, decimal (.), 0, and equals (=) which has a the class span-two that makes it the size of two buttons on the calculator.

CSS
1. *, *::before, *::after {} selects all elements before and after - border box to easily size elements, font, font weight
2. background {} padding and margin - 0, linear-gradient (to right) first color left second color on the right
3. Style our grid .calculator-grid{} display is grid, min height 100vh 100% of pages height, grid-template columns repeat 4(columns) 100px wide, grid-template rows repeat 5(rows) 100px wide minmax(120px, auto) min 120px tall - max auto as large as needed to fit,  and use justify - content so all buttons are touching essentially 
4. .calculator-grid > button {} cursor hovers over our button on calculator it will show a pointer to indicate its clickable, border 1px solid white around the calculator, outline none, background color of calculator is a greyish color, font size is 2rem, 
5. .calculator-grid > button:hover{} higher opacity when you hover over a button on the calculator
6. .span-two {} grid-column : Spans 2 columns 
7. .output {} grid-column: 1 all the way to column -1 (last column), background-color black, display flex: flex-end (right side of container) justify content: space-around elements are slightly closer, flex-direction: column - veritcal display vs horizontal, padding 10px, word-wrap: wraps as the element gets to long, word-break all words,
8. .output .previous-opperand {} smaller than current and slightly different color
9. .output .current-opperand {} larger than previous opperand and slightly darker color 

JavaScript
1. add our data-operation to select each of our operation buttons - data-number same for numbers on calculator - period . acts similar to a number - data-del - data- all - clear - Have data-previous-opperand and data-current opperand to our divs that contain the prior and current opperands 
2. const variables - numberButtons = document.querySelectorAll('[data-number]') constant variable for our numbers, do the same for operationButtons, equalsButton = document.querySelector() Gets a single element - in this case our equals button, do the same for our delete button, all-clear, const previousOperandTextElement and current.
3. How do we store the information? - class Calculator { constructor(previousOperandTextElement, currentOperandTextElement){this.previousOperandTextElement = previousOperandTextElement this.currentOperandTextElement = currentOperandTextElement } - takes all of the inputs and functions for our calculator - Where are we displaying our display text for our calculator - variables sets the text element operands in our calculator class 
4. Define our functions: clear(), delete(), appendNumber(number) - a user clicks a number its added to the screen, chooseOperation(operation), compute(), updateDisplay()
5. clear - removes currentOperand = '' and removes previousOperand = '' (empty strings) and our operation is undefined - add this function to our Calculator class clear all of our inputs and sets the default values of an empty string in the calculator 
6. const calculator = new Calculator(previous, current) creates new calculator
7. number buttons are selected for each (loops over all buttons), each button has an event listener whenever its clicked we add the number inside whatever number is clicked calculator.appendNumber (button.innerText) - then we need to update our display calculator.updateDisplay()
8. updateDisplay() {} take current operand text element = this.currentOperand (display is now updating in live server)
9. appendNumber(number) {} take current operand text element = number selected (can now display a number in the top of calculator) 
10. write our append number function - our current operand = curentOperand.toString() + number.toString() (our numbers are being appended as strings so that JS does not actually add our numbers automatically)

    appendNumber(number) {
        if (number === '.' && this.currentOperand.includes('.')) return // cannot select decimal multiple times 
        this.currentOperand = this.currentOperand.toString() + number.toString()
    }
11. Add our operationButtons.forEach(button=>{ button.EventListener('click, ()=> { calculator.chooseOperation(button.innerText) calculator.updateDisplay()}) }) - selects an operation and updates display 
12. Write our chooseOperation(operation){} set our operation = operation passed in - set previous operand = to our current operand - then we print '' empty string after operation is selected so that the user can put in numbers - if current operand ='' empty string we return - if previous opperand is not equal to an empty string we  call this.compute() updates on calculator screen
13. equalsButton.addEventListener if clicked our equals button will compute the operation and update our display 
14. compute() {} let computation // our result of compute function, const prev and const current, if it is NaN for previous value or our current value then we want to cancel this function - use return, switch (this.operation) { case '+': operation is addition - computation = prev + current break - do not go to other cases break out, case for each operation - subtraction -  computation = prev - current break, multiplication - prev * current, division - prev/current, else statement or default: any time none of these none of these symbols do not match our operations it is invalid we want to just return and not do any operation
15. set our current Operand = computation , this.operation = undefined, this.previousOperan = ''
16. allClearButton.addEventListener - calls the clear function then updates our display
17. delete function - current operand = currentOperand.toString().slice(0,-1) selects all numbers sets it equal to our current operand 
18. delete event listener - calls delete function and updates our display with display function
19. updateDisplay() {} if we have an operation != null then we want to display our previous operand concatenated to our operation 
20. helper function of getDisplayNumber - passed in a number - returns number to a display function - floatNumber = parseFloat(number) if NaN return an empty string - if we do have a number we want our number to use english language to add commas to the number appropriately 
21. the issue with the helper function now is that if we select decimal it cannot be parsed as a Float - problems with 0s after decimal
22. const stringNumber = number.toString() splits string to decimal character inside of it -  const integerDigits = parseFloat( take our string turn it to an array take the first part before a decimal - add the same for numbers after our decimal const decimalDigits = stringNumber.split('.')[1] // [1] indicates after the decimal 
23. let integerDisplay - if isNaN(integerDigits) { integerDisplay is an empty string} else our integerDisplay is integerDigits.toLocaleString('en', {
maximumFractionDigits: 0} ) 
24. if our decimal digits != null then our user did use a period and has some numbers after it - return our integerDisplay.decimalDigits else return our integerDisplay allows you to start with a decimal (.25) or any format
25. updateDisplay() - else after our operation we want the above number (previousOperand) to be an empty string 
