# I earned how to use pure react, with cdnjs


I learned that react is just a library and not a framework. And we can import it via CDNJS and use its functions via Javascript.

In that study I imported React and ReactDOM, like this:



```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react.js" integrity="sha512-bPEAI/w5skhB3Kchsnt+R/e9Bvaije6PJhB5FBy6CRzUC9dB52NS9e7OK2LJQfdOUJdTkIMvA+ioWjEMYv37Jg==" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react-dom.js" integrity="sha512-SYAv6yPI2ytwfZd1nrA0yA/HxtRk+jcIhjm0LIxPHFM7mkcCQo+/YFivrio49aHGOYKE0hYgwmRPZzoXQQmbQw==" crossorigin="anonymous"></script>
```

Then you can use the react functions through the React object.

```html
<script>
   console.log(React)
</script>
```

You can create an element using the function createElement, like this:

```js
var h1 = React.createElement('h1', null, 'Hello h1 React')
```


And to render, you use ReactDOM.render and pass the parameters in order: element and where you will render.

```js
   ReactDOM.render(h1, document.getElementById('app'))
```


The complete example would look like this:


```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <title>Study 01</title>
</head>
<body>

   <div id="app"></div>

   <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react.js" integrity="sha512-bPEAI/w5skhB3Kchsnt+R/e9Bvaije6PJhB5FBy6CRzUC9dB52NS9e7OK2LJQfdOUJdTkIMvA+ioWjEMYv37Jg==" crossorigin="anonymous"></script>

   <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.1.0/react-dom.js" integrity="sha512-SYAv6yPI2ytwfZd1nrA0yA/HxtRk+jcIhjm0LIxPHFM7mkcCQo+/YFivrio49aHGOYKE0hYgwmRPZzoXQQmbQw==" crossorigin="anonymous"></script>

   <script>
   // create elemente
   var h1 = React.createElement('h1', null, 'Hello h1 React')   
   // render element
   ReactDOM.render(h1, document.getElementById('app'))
   </script>
</body>
</html>
```