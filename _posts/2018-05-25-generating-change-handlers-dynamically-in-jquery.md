---
layout: post
title: Generating Handlers Dynamically in jQuery
tags: [js]
---
Not many developers talk about [jQuery](https://jquery.com/) these days as they are probably busy working with frameworks such as [Angular](https://angular.io/) or [React](https://reactjs.org/). Believe it or not though, jQuery is still a very popular framework today. As a matter of fact, in the job listings, [jQuery is still among the top three most used frameworks](https://medium.com/javascript-scene/top-javascript-libraries-tech-to-learn-in-2018-c38028e028e6) (the other two being Angular and React). This means that jQuery is here to stay and many clients still use it today.

A very common challenge that developers face while working with jQuery, is the dynamic generation of handlers to elements. Very often, you'll face the need to attach handlers to a collection of elements. 

Let's assume that you use a for-loop that loops through the collection of elements and initializes the handlers, the thing which is not so obvious is that the handler function context is lost in the loop. In other words, the handler initialization in the following code does **not** work:

```javascript
function initializeChangeHandlers() {
    var numberOfElements = 10;
    for(var i = 0; i < numberOfElements; i++) {        
        $("#element"+i).change(function() {
            console.log("Initialized change handler for element.");
        });
    }    
}
```

This is confusing since the code looks logical and correct. As mentioned earlier, the problem here is that the function context of the handler is lost in the loop. In order to preserve the function context, we need to create a new function that handles the event and then call that function upon binding. The following code works:

```javascript
function initializeChangeHandlers() {
    var numberOfElements = 10;
    for(var i = 0; i < numberOfElements; i++) {        
        $("#element"+i).change(generateChangeHandler());
    }    
}

function generateChangeHandler() {
    return function(event) {
        console.log("Initialized change handler for element.");        
    };
}
```

This simple change solves the issue of dynamically binding handlers to a collection of elements, this is of course not limited to change handlers but to any type of handler.