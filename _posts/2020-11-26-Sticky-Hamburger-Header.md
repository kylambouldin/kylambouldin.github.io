---
layout: post
title: Sticky Hamburger Header
---

Today I will implement a nav bar header for the new site. I want this header to contain the logo on the left and links to the other pages on the right. On a mobile screen, I want the header to collapse down into a hamburger menu. My goal is to not use any Javascript and just used pure CSS. Lets get started!

## Step 1: Add responsive meta tags
Add this piece of code in your `<head>` tags. This allows the screen size to scale correctly on different devices.
```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## Step 2: Create a file to store responsive media sizes
I created a file called `variables.scss` to store any reusable components. I added 3 different media sizes for desktop, tablet, and mobile. I develop my css mobile first, so tend to only use the desktop variable.
```
// Media Sizes
@mixin desktop {
  @media screen and (min-width: 992px) {
    @content;
  }
}

@mixin tablet {
  @media screen and (max-width: 768px) {
    @content;
  }
}

@mixin mobile {
  @media screen and (max-width: 768px) {
    @content;
  }
}
```
This file can also store other variables such as colors and fonts
```
$white: #ffffff;
$black: #000000;
```
## Step 3: Add header files
Create a `header.scss` and a `shared/_header.html.erb` file. This is where the code for the header will live.

If you want to use an image as the logo instead of text, make sure to drop that in your assets/images folder. Next copy the following code into your header.html.erb file and make sure to replace the image tag information with the image you are using. Otherwise replace the whole image tag with the text you want to show on the left side of the navigation menu.

Later we will go through how to connect the links to actual pages.

### header.html.erb
```
<header class="header">
  <a href="" class="logo"><%= image_tag("my-image.png", :alt => "My Company Logo") %></a>
  <input class="menu-btn" type="checkbox" id="menu-btn" />
  <label class="menu-icon" for="menu-btn"><span class="navicon"></span></label>
  <ul class="menu">
    <li><a href="#work">Work</a></li>
    <li><a href="#about">about</a></li>
    <li><a href="#careers">Careers</a></li>
    <li><a href="#login">Login</a></li>
  </ul>
</header>
```

### header.scss
Make sure to import your variables class so that your header can access the media sizes. The css creates a desktop header that is 90px and a mobile header that is 60px.

The media sizes that you created are used by calling `@include desktop { }`
```
@import 'variables';

body {
  margin: 0;
  font-size: 16px;
}

.header {
  position: fixed;
  top: 0;
  z-index: 100;
  height: 60px;
  width: 100%;
  background-color: $white;

  ul {
    margin: 0;
    padding: 0;
    list-style: none;
    overflow: hidden;
  }

  li {
    margin: 0 auto;
    max-width: 50%;
  }

  li a {
    display: block;
    padding: 20px 20px;
    text-decoration: none;
    color: $black;
  }

  li a:hover, .menu-btn:hover {
    opacity: 0.7;
  }

  li a.visted {
    color: $black;
  }

  #sign-up {
    margin-top: 12px;
  }

  .logo {
    float: left;
    height: 100%;

    img {
      height: 60px;

      @include desktop {
        height: 90px;
      }
    }
  }

  .menu {
    clear: both;
    max-height: 0;
    transition: max-height .2s ease-out;
    background-color: $white;
    margin-top: 20px;
    text-align: center;
  }

  .menu-icon {
    cursor: pointer;
    display: inline-block;
    float: right;
    padding: 28px 20px;
    position: relative;
    user-select: none;
  }

  .menu-icon .navicon {
    background: #333;
    display: block;
    height: 2px;
    position: relative;
    transition: background .2s ease-out;
    width: 18px;
  }

  .menu-icon .navicon:before, .menu-icon .navicon:after  {
    background: #333;
    content: '';
    display: block;
    height: 100%;
    position: absolute;
    transition: all .2s ease-out;
    width: 100%;
  }

  .menu-icon .navicon:before {
    top: 5px;
  }

  .menu-icon .navicon:after {
    top: -5px;
  }

  .menu-btn {
    display: none;
  }

  .menu-btn:checked ~ .menu {
    max-height: 300px;
  }

  .menu-btn:checked ~ .menu-icon .navicon {
    background: transparent;
  }

  .menu-btn:checked ~ .menu-icon .navicon:before {
    transform: rotate(-45deg);
  }

  .menu-btn:checked ~ .menu-icon .navicon:after {
    transform: rotate(45deg);
  }

  .menu-btn:checked ~ .menu-icon:not(.steps) .navicon:before,
  .menu-btn:checked ~ .menu-icon:not(.steps) .navicon:after {
    top: 0;
  }

  @include desktop {
    height: 90px;

    li {
      float: left;
    }
    li a {
      padding: 20px 30px;
    }
    .menu {
      clear: none;
      float: right;
      max-height: none;
    }
    .menu-icon {
      display: none;
    }
  }
}

.page-content {
    position: relative;
    padding: 10px;
    top: 60px;


    @include desktop {
      padding: 24px;
      top: 90px;
    }
}
```

### Adjust main content
Now that you have a sticky header, it may overlay on top of your actual content. Let's fix that! The last class in the css file is `.page-content`.
In your application.html layout, cdd this class to a div that surrounds in the yield call:
```
<div class="page-content">
  <%= yield %>
</div>
```

## Test!
Start your rails server and open your localhost to make sure it works. On desktop or wide browser pages, the menu bar should be horizontal. When you shrink the browser page or go into mobile view, you should see the hamburger menu and clicking on it should open up the vertical menu. If everything looks good, push the code to your staging app.
