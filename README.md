# Python-In-Browser
An example of how to use python in the browser using [Brython](https://brython.info/) 
 


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
 
