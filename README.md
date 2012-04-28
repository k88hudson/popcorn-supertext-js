popcorn.supertext.js
====================

Supertext is a text plugin for popcorn.js. It can be used to create subtitles, or add text to any target element on the page. There is a subtle fade in/out transition by default, but Supertext supports custom transition times, container classes, active/inactive states, and styles for inner text.

Supertext is based on the footnote and subtitle plugins, but instead of showing/hiding divs with `style.display`, it handles this with clases and css3 transitions.

How it works (when a target is specified)
-------------
When the plugin is initialized, Supertext creates a div that contains your text and adds it to the target. Between start and end times, Supertext adds an 'active' class to the container. Otherwise it as an 'inactive' class.

**Example:**
```javascript
 var p = Popcorn('#video')
        .supertext({
          start: 3, // seconds
          end: 10, // seconds
          text: 'This is my cool text',
          target: 'mytarget',
        });
```
**Before Plugin Setup**
```html
<div class="mytarget"></div>
```
**After Plugin Setup**
```html
<div class="mytarget">
	<div class="supertext-container supertext-off">This is my cool text</div>
</div>
```
**Between start and end**
```html
<div class="mytarget">
	<div class="supertext-container supertext-on">This is my cool text</div>
</div>
```
The example here uses the default base, active and inactive classes, which are `supertext-container` and `supertext-on`, but you can specify them to be whatever you want. The plugin adds the following styles to the document head:

```css
.supertext-container {
  overflow: hidden;
}
 .supertext-on {
  visibility: visible;
  opacity: 1;
  /* Show */
  -webkit-transition: opacity .5s linear .5s;
  -moz-transition: opacity .5s linear .5s;
  -o-transition: opacity .5s linear .5s;
  transition: opacity .5s linear .5s;
}
 .supertext-off {
  visibility: hidden;
  opacity: 0;
  /* Hide */
  -webkit-transition: visibility 0s .5s, opacity .5s linear;
  -moz-transition: visibility 0s .5s, opacity .5s linear;
  -o-transition: visibility 0s .5s, opacity .5s linear;
  transition: visibility 0s .5s, opacity .5s linear;
}
 .supertext-off > div {
  margin-top: -10000px;
  -webkit-transition: margin-top 0s .5s;
  -moz-transition: margin-top 0s .5s;
  -o-transition: margin-top 0s .5s;
  transition: margin-top 0s .5s;
}
```
The technique is a modified form of a solution presented by [Florent Verschelde](http://fvsch.com/code/transition-fade/test5.html).

The default classes use CSS3 transitions to create fade in and out effects by default. You can specify a custom transition time in milliseconds with the `defaultTransition` option.

Unlike the footnotes/subtitles plugins, Supertext only allows one container per target element to show at a time, so specifying an `end` value is optional if you are adding many Supertexts successively.

How it works (subtitles)
-------------
When a target is *not* specified, Supertext adds your text to a subtitle container. The id of this container is supertext-subtitles-*mediaID* and the class is supertext-subtitles, where mediaID is the ID of your popcorn media element. The following default style is added to the header:
```css
.supertext-subtitles { 
  font-family: "Helvetica Neue", Helvetica, sans-serif;
  text-align: center;
  text-shadow: 0 0 4px #000;
  color: #FFF;
}
```
Adding inner CSS
-------------
If you wish to add styles to the text, you can do with the `innerCSS` or `innerClasses` options. If you are applying classes to subtitles, it is best to write them like this in order for them to have a higher specificity:
```css
.supertext-subtitles .yourclassname {
  ...
}
```

Options
-------------
<table>
  <tr>
    <th>Name</th><th>Default</th><th>Description</th>
  </tr>
  <tr>
    <td>start</td><td>none</td><td>The start time, in seconds</td>
  </tr>
  <tr>
    <td>end</td><td>none</td><td>The end time, in seconds</td>
  </tr>
  <tr>
    <td>text</td><td>none</td><td>The text to display</td>
  </tr>
  <tr>
    <td>defaultTransition</td><td>500</td><td>The transition time for all Supertext instances that use the default supertext-container and supertext-on classes. Note that if this is specified, it will affect ALL Supertext instsances that use the default classes.</td>
  </tr>
  <tr>
    <td>baseClass</td><td>supertext-container</td><td>The class to be applied to the div container for the text, which is added to the target element.</td>
  </tr>
  <tr>
    <td>activeClass</td><td>supertext-on</td><td>The class to be applied to the div container when it is active (between start and end times)</td>
  </tr>
  <tr>
    <td>inactiveClass</td><td>supertext-off</td><td>The class to be applied to the div container when it is active (between start and end times)</td>
  </tr>
  <tr>
    <td>innerCSS</td><td>none</td><td>Inline CSS to be applied to the inner div.</td>
  </tr>
  <tr>
    <td>innerClasses</td><td>none</td><td>A list of classes to be applied to the inner div separated by spaces.</td>
  </tr>
  <tr>
    <td>target</td><td>none</td><td>The target in which to add the Supertext container. If it is not specified, the container will be added to a subtitle div overlayed on the media element (see above)</td>
  </tr>
   <tr>
    <td>callback</td><td>none</td><td>A function to be run at the end of the plugin setup.</td>
  </tr>
</table>


Example with all options:
-------------
```javascript
 var p = Popcorn('#video')
        .supertext({
          start: 3, // seconds
          end: 10, // seconds
          text: 'This is my cool text',
          defaultTransition: 400, //in milliseconds
          baseClass: 'supertext',
          activeClass: 'active',
          inactiveClass: '',
          innerCSS: 'color:red;',
          innerClasses: 'big meta',
          target: 'myTarget',
          callback: function() {
            console.log("Supertext is playing!");
          }
        });
```