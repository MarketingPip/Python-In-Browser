# Python-In-Browser
An example of how to use python in the browser using [Brython](https://brython.info/) 
 

<b>Adding intergers:</b> 

```html
<script src="https://cdn.jsdelivr.net/npm/brython@3/brython.min.js">
</script>
<script src="https://cdn.jsdelivr.net/npm/brython@3/brython_stdlib.js">
</script>
<body onload=brython()>
<script type=text/python>
from browser import document
from random import randint


num1 = 1.5
num2 = 6.3

# Add two numbers
sum = num1 + num2

# Display the sum
test = ('The sum of {0} and {1} is {2}'.format(num1, num2, sum))
document.body.html = (test)  
</script>
```

[Python In Browser Example - JSFiddle - Code Playground](https://jsfiddle.net/271per8h/) 


<b>Input Box Example:</b>

```html
<style>#zone{
    position: absolute;
    background-color:#fff;
    color: #000;
    padding: 5px;
    width: 200px;
    text-align: center;
    border-color: #000;
    border-style: solid;
    border-width: 1px;
}</style>
<script src="https://cdn.jsdelivr.net/npm/brython@3/brython.min.js">
</script>
<script src="https://cdn.jsdelivr.net/npm/brython@3/brython_stdlib.js">
</script>
<body onload=brython()>
<body onload="brython(1)">

<table width="100%">
<tr>
<td width="25%" style="padding-left:3%;">
<h2>Simulate an input() dialog box</h2>
The built-in function <code>input()</code> is <i>blocking</i>: the script
execution stops until the user enters a value. It is implemented with the
(blocking) built-in Javascript function <code>prompt()</code>, which opens a
dialog box that can't be customized.
<p>By design, Javascript doesn't support user-defined blocking functions, so
it's impossible to override <code>input()</code> with a function that would
open a dialog box made of HTML elements (an INPUT tag, "Ok" and "Cancel"
buttons) and stop execution until the user clicks on one of the buttons; the
actions to execute when a value has been entered must be defined in a callback
function.
<p>In this example, we use a generic function <code>dialog()</code> that
handles the entry of an integer value in an INPUT tag. It loops until the
value is a valid integer literal.The comments in the source code explain the
interface of this function.
</td>
<td id="right">
</td>
</tr>
</table>

<script type="text/python">
from browser import document, window, html, bind

def dialog(draw, remove, callback, validate=None):
    """Generic function to handle a dialog box, similar to Javascript
    built-in "prompt".
    draw and remove are the functions that draw and remove the dialog box.
    draw() returns a 3-element tuple : the <input> tag, the "Ok" and "Cancel"
    buttons.
    If validate is provided, it must be a callable that is applied to the
    input value:
    - if validate(value) doesn't raise a ValueError, the dialog box is removed
    by calling remove(), and callback(value) is called.
    - if it raises a ValueError, the dialog box is reset.
    """
    inp, btn_ok, btn_cancel = draw()

    @bind(btn_ok, "click")
    def click(ev):
        v = inp.value
        try:
            if validate is not None:
                validate(v)
            remove()
            callback(v)
        except ValueError:
            dialog(draw, remove, callback, validate)

    @bind(btn_cancel, "click")
    def cancel(ev):
        remove()

    @bind(inp, "keyup")
    def keyup(ev):
        if ev.key == "Enter":
            click(ev)

def draw_dialog():
    """Draw the dialog box, with an INPUT field and buttons "Ok" and "Cancel".
    """
    try:
        zone = document["zone"]
        zone.clear()
    except KeyError:
        zone = html.DIV(id="zone")
        document["right"] <= zone
    msg = html.DIV("Enter an integer")
    inp = html.INPUT()
    buttons = html.DIV(style={"text-align": "center"})
    btn_ok = html.BUTTON("Ok")
    btn_cancel = html.BUTTON("Cancel")
    buttons <= btn_ok + btn_cancel
    zone <= msg + inp + html.BR() + buttons

    # center zone in page
    zone.left = int((window.innerWidth - zone.offsetWidth) / 2)
    zone.top = int(window.innerHeight / 10)

    # set focus on the <input> tag
    inp.focus()

    # return the 3-element tuple expected by dialog()
    return inp, btn_ok, btn_cancel

def remove():
    """Remove the dialog box."""
    document["zone"].clear()

def show(value):
    """Called when user entered a valid value."""
    document["zone"].clear()
    document["zone"] <= f"Integer value entered: {value}"

def check_int(value):
    """Try to convert value (a string) into an integer. Raises ValueError
    if value is not an integer literal."""
    int(value)

dialog(draw_dialog,
       remove,
       validate=check_int,
       callback=show)
</script>
```

[Python In Browser - Input Box - JSFiddle - Code Playground](https://jsfiddle.net/jh9kqw5b/) 
 
 
