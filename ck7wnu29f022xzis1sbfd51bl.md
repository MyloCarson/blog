## Adding dark mode to a static website.

Since Twitter dark mode came up, I just loved the idea behind dark mode. I have switched all my applications that supports dark mode, so I decided to add it to my  [portfolio](https://seunakanni.me) .

I am sure if you are reading this article, you probably have worked on your website in "light-mode" and you just want to add more swagger by "adding dark-mode".

Let's get started already!

1: Create CSS classes ".light-mode" and ."dark-mode". These are the most important classes we would need in order to successfully switch modes off/on using Javascript.
2: I like to think the major difference between dark-mode and light-mode is color based i.e "background", "background-color", "color" for texts, "fill" for svg path. 

Do feel free to use any color for "light-mode". But for dark mode, let's proceed to create CSS variables.

```
 :root {
     --dark-background : #15202B;
     --dark-element-background: #192734;
     --dark-mode-link: #1DA1F2;
     --white: #fff;
 }
``` 
3: Next, structure the HTML page. I would suggest impplementing HTML structure like this,

```
<!DOCTYPE html>
<html lang="en">

<head>
 <!-- head content goes here -->
</head>
<body>
   <div id="parent" >
      <!-- other contents to be displayed goes here -->
    </div>
</body>
</html>

``` 
This structure makes sure there is a parent, which would effectively make it possible to override any style of its child/children at any time.

4: Dive into your stylesheet, and follow this stylus
 (i) for every element selector that has a background, or background-color, or color, or fill, you will precede a ".light-mode" before such selector.

Example:

```
.light-mode{
    background: #dae3e7;
}
.light-mode .header {
    background: #f5f5f5;
}
.light-mode .profile_name {
    color: #b0b7bf;
}

.light-mode a{
    color: #3740ff;
}

.light-mode .content h1 , .light-mode .content h2 {
    color: black;
}
.light-mode .education_item span,
.light-mode .langauge_item span {
    color: dimgray;
}
.light-mode .section, .light-mode .aside{
    background: var(--white);
}

``` 
(ii) for the same element selector in (i) that has a background, or background-color, or color, or fill, you will precede a ".dark-mode" before such selector. 

Note: It is important to note that only "dark-mode" colors  are used here.

```
.dark-mode{
    background: var(--dark-background);
}
.dark-mode .header {
    background: var(--dark-element-background);
}
.dark-mode .profile_name {
    color: var(--white);
}
.dark-mode a{
    color: var(--dark-mode-link);
}
.dark-mode .content h1 , .dark-mode .content h2 {
    color: var(--white);
}
.dark-mode .education_item span,
.dark-mode .langauge_item span {
    color: var(--white);
}
.dark-mode .section, .dark-mode .aside{
    background: var(--dark-element-background);
}

``` 
Great! Now, there are stylings for both "dark-mode" and "light-mode" of every elements on the HTML page.

5: You can go ahead to add either "light-mode" or "dark-mode" as the default CSS class for the parent div. Also, create an element, which will act as a toggle button to switch modes.
 

```
<!DOCTYPE html>
<html lang="en">

<head>
 <!-- head content goes here -->
</head>
<body>
   <div id="parent" class="light-mode">
      <!-- other contents to be displayed goes here -->
     <button id="bulb" > Click to Toggle Mode</button>
    </div>
</body>
</html>

``` 
6: Create script tag in your HTML, or better still create an index.js, then link it to your HTML. Next, we write a toggle function to toggle between modes. Also, we can add some spice by changing the modes per time of day, i.e "light-mode" during the day and "dark-mode" at night.

Dive in!

```
document.addEventListener('DOMContentLoaded', function(event) {
  let isLightMode = true;
  const parent = document.getElementById('parent');

  //find current hour of the day
  const hour = new Date().getHours(); //get hours of the day in 24Hr format (0-23)
  if (hour >= 18) {
    // set night mode at night
    isLightMode = false;
    parent.className = 'dark-mode';
  } else {
    // set light mode during the day
    isLightMode = true;
    parent.className = 'light-mode';
  }

  //get toggle button and add click event listener
  const bulb = document.getElementById('bulb');
  bulb.addEventListener('click', function() {
    if (isLightMode) {
      isLightMode = false;
      parent.className = 'dark-mode';
    } else {
      isLightMode = true;
      parent.className = 'light-mode';
    }
  });
});

``` 
 
Now, you see adding dark-mode to your static website wasn't that hard!
Enjoy!! 😎

 [Here](https://seunakanni.me)  is a working example of my implementation.