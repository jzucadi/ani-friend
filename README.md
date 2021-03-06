```
npm install --save ani-friend
```

# Animation Friend

This tool will reveal an area when in view and allow you to control
how the container and any children elements appear.

As the user scrolls, it can lazyload images when they’re at
one screen height lower.

There are built in animation presets, or you can build your own.

![Example Zoom and Wipe motion](http://ua5.co/67e2fb2440aa/wipe-and-zoom.gif)
![Example Text Motion](http://ua5.co/60c9d83d5ad8/ezgif-1-6b4c24069a4e.gif)

### Usage

```javascript
import { Ani } from 'ani-friend'

const ani = new Ani()

// If new elements that are animation groups get added dynamically,
// calling this will add any new elements with an ani attribute
ani.update()
```

```html
<section ani="basic-appear">
    <img ani-child load-src="/images/classes.jpg" />
    <img ani-child ani-child-order="4" ani-preset="fade-down" load-src="/images/marketing.jpg" />
    <img ani-child ani-child-order="2" ani-preset="wipe-down" load-src="/images/marketing.jpg" />
    <div ani-child ani-preset="fade" ani-child-order="1" style="background: green; display: inline-block">
        <img ani-child ani-child-order="3" ani-preset="wipe-left" load-src="/images/marketing.jpg" />
    </div>
    <img ani-child ani-child-order="1" ani-preset="fade-up" load-src="/images/marketing.jpg" />
    <img ani-child ani-preset="class-my-class-name" load-src="/images/marketing.jpg" />
</section>
```

Add this to your styles in the head, so all elements are hidden prior to DOM loading:

```css
[ani],
[data-ani] {
    opacity: 0;
}
```

### HTML Attributes

You may preface each attribute with `data-` if necessary.
- `ani`: Takes a string for the parent preset name
- `ani-in-view-trigger-percent`: attribute for ani parents. Decimal - when to reveal the group. When scrolling, a lower number will show the item sooner than higher numbers.
- `load-src`: add to image tags in lieu src attributes, and it will lazy load
- `ani-child`: Defines this node to be a child
- `ani-child-order`: you can sequence the order of nodes that come in
  - If you leave `ani-child-order` blank, the el takes the dom order, but ranks lower than nodes that are set
- `ani-delay-speed`: in seconds, it gets multiplied by its order or index
- `ani-speed`: in seconds, the duration that the animation occurs in in
- `ani-move-distance`: in pixels, for x/y transforms on the fade preset
- `ani-preset`: Micro animations for individual objects.
  -  `basic-appear` comes with: `fade`, `wipe`, `zoom`, `text`, and `class`
  - Then you may append a direction `-up`, `-down`, `-left`, `-right`; Zoom: `-in` and `-out`. Text: `-line-mask` and `-line-appear`.
  -  If you select class, you'll need to append a class name
    - Example: `ani-preset="class-my-class-name"` — this will add `my-class-name` to the element's class list in lieu of an appear function.
  - For zoom, the element must be in a wrapper with an `overflow: hidden` that will crop the target.
  - For text, it will break the text in an element into lines so they can be individually animated.
- `ani-text-line-y-offset`: Positive or negative number to  adjust the starting offset of each line of text.
- `ani-text-line-delay-speed`: Number to adjust the delay speed that each line appears at. It is multiplied by the line index.
- `ani-ease`: String for ease property - see [gsap ease visualizer](https://greensock.com/ease-visualizer). Example: `ani-ease="Power4.easeInOut"`.

### Example animation Group Action

```javascript
AniGroupActions['basic-appear'] = (el, children) => {
    el.style.opacity = 1
    el.style.transition = 'none'
    children.forEach((item, index) => {
        let preset = ''
        if (item.hasAttribute('ani-preset')) {
            preset = item.getAttribute('ani-preset')
        }
        const ani = new AniElement(item, index, preset)
        ani.appear()
    })
}
```
