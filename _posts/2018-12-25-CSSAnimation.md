---
layout: post
title: "CSS Animation"
categories: CSS Animation
published: true
---
Notes on CSS Animation.

# #Basics
- Its movement with:
    - Transition properties.
    - Animate Property + Key frames

- Syntax:

    ```scss
    .element {
        transition: [property] [duration] [ease] [delay];
    }
    ```

- Animatable Properties:
    - font-size
    - background-color
    - width
    - left
    - **position**
    - **scale**
    - **rotation**
    - **opacity**
    - ~~display~~
    - ~~font-family~~
    - ~~position~~

- To Trigger Animation:
    - Hover Pseudo Class
    - Class Change
    - New elements:
        - With animation already on them.


## #pink-box Transition
```html
<div class="tigger">
    <div class="box"></div>
</div>

<style>
    body {
        padding: 50px;
    }

    .trigger {
        width: 200px;
        height: 200px;
        border: 20px solid #999;
        background: #ddd;
    }

    .box {
        display: inline-block;
        background: pink;
        width: 200px;
        height: 200px;
        transition: transform 300ms cubic-bezier(0, 0.47, 0.32, 1.97);
        pointer-events: none;
    }

    .trigger:hover .box {
        transform: translate(200px, 150px) rotate(20deg);
    }

    /*For JS*/
    .trigger.clicked .box {
        transform: translate(200px, 150px) rotate(20deg);
    }
</style>

<!--With JS-->
<script>
    $('.trigger').on('click', function() {
        $(this).toggleClass('clicked');
    });
</script>
```

# #Animation Property & Keyframes

- Keyframes:
    - On Root
    - Syntax: Keyframe
        - In the Root

    ```css
    @keyframes [name] {
        from {
            [styles];
        }
        to {
            [styles];
        }
    }
    ```

    - Syntax: Animation
        - In the element

    ```css
    .element {
        animation: [keyframe_name] [duration] [timing-function] [delay] [iteration-count] [direction] [fill-mode] [play-state];
    }
    ```

- Example:

```html
<div class="box">
</div>

<style>
    body {
        padding: 50px;
    }

    .box {
        display: inline-block;
        background: pink;
        width: 200px;
        height: 200px;
        position: relative;
        
        animation-name: myframes;
        animation-duration: 2s;
        animation-timing-function: ease-in-out;
        animation-delay: 0s;
        animation-iteration-count: infinite;
        animation-direction: normal;
        animation-fill-mode: forwards;
        animation-play-state: running;
    }

    @keyframes myframes {
        0% {
            height: 200px;
        }
        /*Pause 30-70*/
        30%, 70% {
            height: 400px;
        }
        100% {
            height: 600px;
            background: orange;
        }
    }
</style>
```