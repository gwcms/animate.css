## Usage with Javascript

You can do a whole bunch of other stuff with animate.css when you combine it with Javascript. A simple example:

```javascript
const element = document.querySelector('.my-element');
element.classList.add('animate__animated', 'animate__bounceOutLeft');
```

You can detect when an animation ends:

```javascript
const element = document.querySelector('.my-element');
element.classList.add('animate__animated', 'animate__bounceOutLeft');

element.addEventListener('animationend', () => {
  // do something
});
```

or change its duration:

```javascript
const element = document.querySelector('.my-element');
element.style.setProperty('--animate-duration', '0.5s');
```

You can also use a simple function to add the animations classes and remove them automatically:

```javascript
const animateCSS = (element, animation, prefix = 'animate__') =>
  // We create a Promise and return it
  new Promise((resolve, reject) => {
    const animationName = `${prefix}${animation}`;
    const node = document.querySelector(element);

    node.classList.add(`${prefix}animated`, animationName);

    // When the animation ends, we clean the classes and resolve the Promise
    function handleAnimationEnd(event) {
      event.stopPropagation();
      node.classList.remove(`${prefix}animated`, animationName);
      resolve('Animation ended');
    }

    node.addEventListener('animationend', handleAnimationEnd, {once: true});
  });
```

And use it like this:

```javascript
animateCSS('.my-element', 'bounce');

// or
animateCSS('.my-element', 'bounce').then((message) => {
  // Do something after the animation
});
```

If you had a hard time understanding the previous function, have a look at [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const), [classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList), [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), and [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).


Add observer for element to fire animation after scrolled into view:

```javascript
document.addEventListener("DOMContentLoaded", function() {
    document.querySelectorAll('.animate__animated').forEach(function(element) {
        console.log(element.classList);
        
        const classes = element.classList;
        for (let i = 0; i < classes.length; i++) {
            if(classes[i] != 'animate__animated' && classes[i].indexOf('animate__') != -1){
                element.dataset.animationid = classes[i];
                element.classList.remove(classes[i]);
            }
        }            
    });
});
		
		
document.addEventListener("DOMContentLoaded", function() {
    const observer = new IntersectionObserver(entries => {
	entries.forEach(entry => {
	    if (entry.isIntersecting) {
		const animationClass =  $(entry.target).data('animationid'); 


		if (animationClass) {
		    entry.target.classList.add(animationClass);
		}
		observer.unobserve(entry.target); // Stop observing after animation is triggered
	    }
	});
    });

    document.querySelectorAll('.animate__animated').forEach(div => {
	observer.observe(div);
    });
});
```
