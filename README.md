popcorn.supertext.js
====================

Supertext is a text plugin for popcorn.js based on the footnote and subtitle plugins. Instead of showing/hiding divs with `style.display`, it shows/hides text by adding and removing an "active" class.

How it works (when a target is specified)
-------------
When the plugin is initialized, Supertext creates a div that contains your text and adds it to the target. Between start and end times, Supertext adds an 'active' class to the container.

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
	<div class="supertext-container">This is my cool text</div>
</div>
```
**Between start and end**
```html
<div class="mytarget">
	<div class="supertext-container supertext-active">This is my cool text</div>
</div>
```
The example here uses the default base and active classes, which are `supertext-container` and `supertext-active`, but you can specify them to be whatever you want. The plugin adds the following styles:

```css
.supertext-container { 
  -moz-transition: opacity 0.5s ease;
  -webkit-transition: opacity 0.5s ease;
  transition: opacity 0.5s ease;
  opacity: 0; height: 0; overflow: hidden;
}
.supertext-active { 
  -moz-transition: opacity 0.5s ease;
  -webkit-transition: opacity 0.5s ease;
  transition: opacity 0.5s ease;
  opacity: 1; height: auto;
}
```

The default classes use CSS3 transitions to create fade in and out effects by default. You can specify a custom transition time in milliseconds with the `defaultTransition` option.

Unlike the footnotes/subtitles plugins, Supertext only allows one container per target element to show at a time, so specifying an `end` value is optional if you are adding many Supertexts successively.

How it works (subtitles)
-------------
When a target is *not* specified, Supertext adds your text to a subtitle container. The ID of this container is supertext-subtitles-*mediaID*, where mediaID is the ID of your popcorn media element. The following default style is added to the header:
```css
#supertext-subtitles-mediaID { 
  font-family: "Helvetica Neue", Helvetica, sans-serif;
  text-align: center;
  text-shadow: 0 0 4px #000;
  color: #FFF;
}
```
Overwriting CSS
-------------
If you wish to overwrite/modify the default styles, you should do so like this:
```css
#yourtarget .supertext-container { 
 ...
}
#yourtarget .supertext-active { 
  ...
}
#supertext-subtitles-mediaID .supertext-container { 
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
    <td>defaultTransition</td><td>500</td><td>The transition time for all Supertext instances that use the default supertext-container and supertext-active classes. Note that if this is specified, it will affect ALL Supertext instsances that use the default classes.</td>
  </tr>
  <tr>
    <td>baseClass</td><td>supertext-container</td><td>The class to be applied to the div container for the text, which is added to the target element.</td>
  </tr>
  <tr>
    <td>activeClass</td><td>supertext-active</td><td>The class to be applied to the div container when it is active (between start and end times)</td>
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
          target: 'myTarget',
          callback: function() {
            console.log("Supertext is playing!");
          }
        });
```