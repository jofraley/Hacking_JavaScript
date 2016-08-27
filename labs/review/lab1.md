###Review HTML, CSS, and JavaScript

This lab covers the basics of creating your first web page.  Take this starter page and play around with modifying the HTML, CSS, and JavaScript.  Use the resource [W3School](http://w3schools.com) to learn what is possible

1. Copy and paste the code below into a new [jsbin.com](http://jsbin.com).

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <script>
        function myFunction() {
            document.getElementById("heading").innerHTML = "Hello World";
        }
    </script>
  </head>
  <body>
  <style>
    h1{
        color:  orange;
        text-align:  center;
        padding:  21;
    }
    p{
        font-family:  cursive;
        font-size:  20px;
    }
  </style>    
  <h1 id="heading">this is a heading</h1>
  <p>this is a paragraph</p>
  <table>this is a table</table>
  <form>this is a form</form>
  <button type="button" onclick="myFunction()">Click Me!</button>
  </body>
  </html>
  ```

2. The JSBin `Output` a web page with basic text and a button and when you click the button the heading text changes to Hello World.

Your app should look something like this:
 * [Code](index.html)
 * [Live App](http://jofraley.github.io/Hacking_Javascript/labs/review/index.html)

###Bonus

* Experiment with different basemaps such as `topo` or `gray`.
* Run the code locally on your machine. Eventually if your app gets larger you'll want to migrate it from JSBin to your desktop.
