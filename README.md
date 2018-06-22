# SyneWav Custom Screens
HTML templates for custom SyneWav screens

## Overview
The `src` directory contains the base template. The player automatically applies the `default.css` file to the screen during runtime. Any styling in this file can be overridden as your custom stylesheets will be loaded afterwards.

The `examples` directory contains some fully built screens to show different ways of building screens.

## Useful CSS
The default styling contains a simple grid system that can be useful for sizing and alignment. These classes can be overridden if desired in your custom CSS.

`.grid` sets a 20x20 grid that is 100% width and 100% height.

`.box` a utility to set the element for positioning on the grid.

`.bw-{number}` sets the width of the box on the grid. Default available in increments of 5.

`.bh-{number}` sets the height of the box on the grid. Default available in increments of 5.

A simple example of this use is to create a 2 column layout that is 70/30.
```
<div class="grid">
    <div class="box bw-70 bh-100"></div>
    <div class="box bw-30 bh-100"></div>
</div>
```

To break the 2nd column into a top and bottom, you can add further elements with the same methods.
```
<div class="grid">
    <div class="box bw-70 bh-100"></div>
    <div class="box bw-30 bh-100 grid">
        <div class="box bw-100 bh-50"></div>
        <div class="box bw-100 bh-50"></div>
    </div>
</div>
```

## Considerations
While it is possible to load external resources through the HTML or the css, these files will not be downloaded and cached on the devices. Normally if a device is offline, it can continue to play the content that is loaded on the device. If the device is offline and loads external resources, it may not be able to maintain the cache of those external files.

Sometimes this is not an important factor. For instance, if you are displaying a news feed, the device will always need to be connected in order to load the news feed.

If you are loading external resources, please be aware of the extra network load that can happen with the size of files and number of devices.

## Screen Templates
Screen Templates define layouts of screens. Regions of a screen are defined by id's on elements. Each id will give a named region that a component (slider, clock, feed, static resource) can be placed. Each screen has one Screen Template.

__Important__
* A screen template must be wrapped in a `<div></div>`. (A single `div` container inside the body of the page.)
  * This container will always fill the screen completely.
* Regions are defined with the `id` attribute.
  * An `id` has no inerit value or behavior
    * A playlist screen _may not use all defined regions_.
    * The region definition will define the behavior.
    * No behavior is defined by the template.

## Feed Templates
Feed templates are applied to feed components inside of regions in the Screen Template. The data that is read from the feed is directly applied to templates. During development of screens, you will likely want to place anticipated values inside the HTML instead of using template strings.

Directly map the values of the feeds to areas of the feed template using dot notation. For example, using the following JSON feed:
```
{
    "fruit": [
        {
            "type": "Banana",
            "color": "yellow"
        },
        {
            "type": "Apple",
            "color": "red"
        },
        {
            "type": "Orange",
            "color": "orange"
        }
    ]
}
```
You can display the data into a page using:
```
<div class="fruit">
    A {fruit.0.type} is {fruit.0.color}.
</div>
<div class="fruit">
    A {fruit.1.type} is {fruit.1.color}.
</div>
<div class="fruit">
    A {fruit.2.type} is {fruit.2.color}.
</div>
```

Additionally, iterable structures in the feeds can be defined within an iterable structure:
```
<div class="items">
    {{@fruit::
    <div class="fruit">
        A {type} is {color}.
    </div>
    }}
</div>
```
The `{{}}` defines the iterable area. The `@name::` defines the location of the iterable data. __NOTE:__ The iterable areas are not currently nestable.

If you remit the path to the iterable data, it will by default iterate the parent of the data. `{{@::}}`

## Components
Components are placed into regions and are not included in any templates. Details for each component can help to mock your templates during development. Since these elements and styles are not used in the final output, they should only be used to make a mock of the screen and removed before final placement.

### Clocks
Available formatting for the date/time is similar to PHP. The clock displays the time relative to the device's time. Timezones and formats are set as well as Daylight Savings Time.

Formatting options available here: [http://php.net/manual/en/function.date.php](http://php.net/manual/en/function.date.php).

Example HTML:
```
<div id="clock">5:00 pm</div>
```

### Slider
Sliders are where the selectable image/video sequences are placed. To replicate the slider component, set the background of the slider area to an example image and set the background strategy to either `contain` or `cover`.

__Important:__ A slider inside of a screen must contain the `id` of `main`. This requirement will be removed in the future.

Example HTML:
```
<div id="main"></div>
```

Example CSS:
```
#main {
    background-image: url(placeholder.jpg);
    background-size: contain;
    background-repeat: no-repeat;
}
```

### Feeds
Feed components are defined by the Feed Templates outlined above. There is no default layout for these.

### Static Resource
Static resources are generally images. Images can be linked in the CSS or with an actual `<img>` element in the template.

## Assets
Each Screen Template and Feed Template can be assigned Assets. Assets are CSS, images, or other static files that will be loaded on the players whenever those screens are played. This allows you to do separation of desired CSS files.

An example would be if you want a logo to be loaded every time a specific feed is used. That logo will be set as an asset on the feed and will automatically be loaded whenever that feed is contained within a screen.
