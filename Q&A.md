# Q&A


## Overview Description

Reminder: here you can find an overview description of the code:

[Robot Programming Details](https://github.com/HackYourFuture/RobotApp/blob/master/robot-0/README.md)

## Jason's questions

**Q1.** I don't think I understand functions. I am also not sure what is the difference between inserting something in () and otherwise. Also, does "render" mean anything at all

**A1.** Consider this **function** from [high-school algebra](https://www.mathplanet.com/education/algebra-2/how-to-graph-functions-and-linear-equations/functions-and-linear-equations) classes:

> 𝑓(x) = x + 7
>
> _if x = 2 then_
>
> 𝑓(2) = 2 + 7 = 9

The value of the function 𝑓(x) is dependent on the value you supply for its argument x. (Instead of the term 'argument', we sometimes use the word 'parameter').

Here is the equivalent JavaScript function:

```js
// function definition
function f(x) {
    return x + 7;
}

// call the function and log its value for x = 2
console.log(f(2));  // -> 9
```

During execution the value of x in the function body (the part between the curly braces) is substituted with the value 'passed' during the function call.

```js
// function definition
function renderToolbar(root) {
    // Why is there now "root" in ()
}
```

Of course, in JavaScript we do more than just work with math functions. In this example we have defined a function that expects a DOM element as its argument. A couple of lines earlier in the code we call the `renderToolbar` function:

```js
// get a reference to the DOM element with id of 'root'
const root = document.getElementById('root');  // (1)
// call the renderToolbar function and pass the reference just obtained as a parameter
renderToolbar(root);  // (2)
```

**(1)** First, we obtain a reference to the DOM object with the `id` of `root`. This happens to be a `div` that we included in the `index.html` file. All the HTML `tags` that we create dynamically in our JavaScript will ultimately be hooked up to the root `div`.

**(2)** Next, we call the `renderToolbar` function, supplying the `root` value from (1) as its argument. For this particular call to `renderToolbar` all occurrences of the `root` parameter are effectively replaced by the value that was passed as an argument. It is as if a local `let` was defined inside the `renderToolbar` function like this:

```js
function renderToolbar() {
    let root = ...  // replace ... with the value of root from (1)
}
```

So, arguments in a function can be thought of as having the same effect as local `let` variables in that function.

> **render**: From [dictionary.com](http://www.dictionary.com/browse/render?s=t):
>
> verb (used with object)<br>
1. to cause to be or become; make.

This term is used a lot in app development to indicate the notion of creating a visual representation on screen or in print of application data.

More info:

- [Functions](https://github.com/HackYourFuture/JavaScript/blob/master/fundamentals/functions.md)
- [Basic DOM manipulations](https://github.com/HackYourFuture/JavaScript/blob/master/fundamentals/DOM_manipulation.md)

**Q2.** Unsure why "document"

```js
const root = document.getElementById('root');
```

`document` is a global variable that is created by the browser at start-up and is available to all JavaScript programs and gives us access to the DOM. The DOM (Document Object Model) is a JavaScript 'view' of the HTML structure of the web page and allows you to  dynamically query and modify it.

More info:

- [Basic DOM manipulations](https://github.com/HackYourFuture/JavaScript/blob/master/fundamentals/DOM_manipulation.md)

**Q3**. What is setAttribute? Does this result in `<div id="toolbar"> </div>`?

 ```js
 toolbar.setAttribute('id', 'toolbar');
 ```

**A3**. All name-value pairs that are specified in an opening HTML tag are called 'attributes'. Thus, in the example `id` is the name of an attribute and `"toolbar"` is its value.

The `setAttribute` method can be used to programmatically set an attribute on a DOM element.

In general, to find out what a DOM method does, just google for  'mdn setAttribute' (or replace 'setAttribute' with whatever method you want to know more about).

**Q4.** I think that addEventListener is an instruction to the computer to look for this event, in this case, "click".  But this is a string here, does that translate to a mouse-click?

**A4**. The `addEventListeners` method adds an 'event listener' for the specified event type to the DOM element on which it is called. In the example below it is called on the `turnLeftButton`, which was previously created as a `button` element. The type of event to listen for (a string value) is specified as the first argument of the `addEventListener` method. In this example below, we want to listen for the `click` event.

```js
turnLeftButton.addEventListener('click', function () {
    turn('left');
});
```

Whenever the `turnLeftButton` is clicked it sends an event to all its event listeners that have registered for the `click` event.

The second argument of the `addListener` method is a function that is called whenever the event is triggered. This function is typically called an 'event handler' (it is also called 'callback' function, i.e. it is called back at any time the event is triggered).

There are may other events that can be listened for. For buttons, you are normally only interested in the `click` event. But a button for instance also generates `mousedown` and `mouseup` events, which can sometimes be useful.

For more info, google 'mdn addEventListener'.

**Q5**. What is then the the point of function (), and then does the "turn("left")" tie back into the function near the bottom?
If so, does it actually matter whether the function is above or below?

**A5.** As explained in A4, the 'anonymous' function is an event handler that is called by the browser whenever the 'click' event for the corresponding button occurs. Our event handler function then calls `turn('left')` which causes our robot to take a left turn. This happens each time the button is clicked for as long as our program is running.

It does not matter whether the `turn` function is defined above or below the event handler. By the time the first `click` event occurs the `turn` function will already be fully defined.

**Q6.** I don't understand `board[robot.y][robot.x]`.

```js
board[robot.y][robot.x] = 'R' + trailIndicators[robot.dir];
```

**A6.** The `robot` object was defined earlier in the code as follows:

```js
const robot = {
    x: 0,
    y: 0,
    dir: 'up'
};
```

It represents the 'state' of the robot. The robot has a position (x, y) and a (compass) direction `dir` (`up`, `down`, `left`, `right`). This is all we need to position the robot on the board.

The board was defined as follows:

```js
const board = [
    ['T', 'T', '.', 'F'],
    ['T', '.', '.', '.'],
    ['.', '.', '.', '.'],
    ['R', '.', '.', 'W']
];
```

To access a cell of the board we first need to specify an index to select a row. This would be our y-axis. Next, within a row we need to select an element. This would be our x-axis. The `robot.x` and `robot.y` values represent respectively the x and y coordinates of the current robot position.

We could achieve the same results with:

```js
// board[robot.y][robot.x] = 'R' + trailIndicators[robot.dir];
const robotRow = board[robot.y];
robotRow[robot.x] = 'R' + trailIndicators[robot.dir];
```


**Q7.** why? I think the result of the console.log is just so that it prints this.

```js
console.log('rendering');
```

**A7.** That's true. But because it happens as a result of pressing a button, it could be useful to see in the console that our function is being called at the right time. This is just for debugging purposes. In a production application you would not want any console.log output to be produced.

**Q8.** I understand that here you want the row to be board.length -1 because the board is 4x4; you want -1 because it starts from 0-3, so you want 4-1. I also understand "row >=0, row--" which means that as long as row is greater or = 0, subtract one. Subtract one from what, though? Board length?

```js
for (let row = board.length - 1; row >= 0; row--) {
    // ...
}
```

**A8.** Normally you would see a loop index variable (which in this case is `row`) go from zero to some length minus 1. In this case however, because we need to draw the board from the top row down we need to start at the row with the highest index (`board.length - 1`) and then work our way down to the row with index zero.

If we used a loop going up from zero to `board.length-1` the board would be drawn upside down.

**Q9.** Why is `row` in `[]`?

```js
const cells = board[row];
```

**A9.** `row` is the loop index variable. Suppose `row` === 1. Then the value of `board[1]` is `['T', '.', '.', '.'],`. This becomes the value of `cells`.

The encompassing `for` loop iterates through the rows from top to bottom. The corresponding index variable `row` goes from `board.length-1` (`=== 3`) to `0`, dealing with a row at a time.


**Q10.** Why the blank?

```js
const elem = document.getElementById('board');
elem.innerHTML = '';
```

**A10.** `innerHTML` is a property available on DOM elements that allow you to set or get the inner HTML of an element as a text string. For instance, the `innerHTML` from this `div`:

```html
 <div id="toolbar">
    <button>TURN-LEFT</button>
    <button>MOVE</button>
    <button>TURN-RIGHT</button>
</div>
```

is this string:

```js
"<button>TURN-LEFT</button><button>MOVE</button><button>TURN-RIGHT</button>"
```

By setting the `innerHTML` property of the `board` to the empty string we effectively clear out all previous content of the board. This is necessary because after each `move` and `turn` the board is rerendered. If we did not clear out the contents first, we would get a new board appended to the previous boards. This can be demonstrated by commenting out the line `elem.innerHTML` and replaying the game.