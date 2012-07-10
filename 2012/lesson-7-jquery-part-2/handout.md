# jQuery - Part 2

## Topics

* Performance
* AJAX
* Event delegation
* Animations
* Plugins
* Utility functions

## Performance

Here are a few best practices and tips for getting the best performance out of jQuery.

### Reduce $() calls

All too often, you will see jQuery like the following:

    $('.elem').css({ 'background-color' : red });
    $('.elem').text('Lorem ipsum');
    $('.elem').fadeOut();

The issue with this is that every time you call `$('#elem')`, jQuery has do some work to get the element(s) and return the jQuery object that contains them. There are a few ways you can make sure to do this only once:

#### Chaining

Chain methods so they all use the same jQuery object:

    $('.elem').css({ 'background-color' : red }).text('Lorem ipsum').fadeOut();

#### Caching

Cache the jQuery object in a variable:

    var $elem = $('.elem');

    $elem.css({ 'background-color' : red });
    $elem.text('Lorem ipsum');
    $elem.fadeOut();

### Optimize selectors

Performance can vary based on which selector you use. Consider the following HTML:

    <ul id="list">
        <li class="item">Item 1</li>
        <li class="item">Item 1</li>
        <li class="item">Item 1</li>
        <li class="item">Item 1</li>
    </ul>

    <p id="some-para" class="some-para">Lorem ipsum dolor sit amet, consectetuer adipiscing elit.</p>

Say you wanted to select all of the list items. There are multiple ways to do so:

    $('li')
    $('.item')
    $('li.item')
    $('#list li')
    $('#list .item')
    $('#list').find('li')
    $('#list').children('li')
    $('li', $('#list'))
    ...

That's a lot of options, but which is fastest? `$('li')`

However, if there are other `li` elements on the page you don't want to select, you'll need to be more specific. In that case, `$('.item')` will be ideal.

Say you want the paragraph. You could try:

    $('p')
    $('#some-para')
    $('.some-para')
    ...

But which is fastest? `$('#some-para')`

#### Rules of thumb:

* For a unique element, using an id is always fastest - `$('#some-id')`
* For 2 or more elements, if you don't need to be specific, use the tag name - `$('div')`
* If you need to get a specific set of elements, put a class on them - `$('.some-class')`
* Avoid attribute and pseudo selectors - `$('input[type=text]')` and `$(':hidden')`

#### Further Reading / Resources

[24 Ways article](http://24ways.org/2011/your-jquery-now-with-less-suck) - Article about jQuery selector performance.

[jsPerf](jsperf.com) - Tests performance of input JavaScript.

A few jsPerf examples out in the wild:

* [id vs class vs tag vs pseudo vs. attribute selectors](http://jsperf.com/id-vs-class-vs-tag-selectors/2)
* [jQuery Selector Perf - Right-to-Left](http://jsperf.com/jquery-selector-perf-right-to-left/91)

Google "jsperf jquery selector speed" for more.


### Event delegation

More on this later.

## AJAX

$.ajax
success and failure callbacks

### Convenience methods

$.get
$.post
$.getJSON

## Event delegation

when to use
* dynamic elements
* binding events to parent instead of myriad elements

$('selector').on('click', fn)
vs
$('selector').on('click', 'selector', fn)

## Animations

$.animate
callback
queue?

## Plugins

what they are
how to use
examples:
* nivo slider
* twitter bootstrap
* datatables?
* jquery ui
how to create?

## Utility functions

## $.fn.each

> Iterate over a jQuery object, executing a function for each matched element.

    <ul>
        <li>apples</li>
        <li>oranges</li>
        <li>bananas</li>
        <li>grapes</li>
        <li>strawberries</li>
    <ul>

    $('li').each(function (index, element) {
        console.log( $(this).text() );
    });

Output:

    apples
    oranges
    bananas
    grapes

### $.each

> A generic iterator that can iterate over objects and arrays.

    var list = ['apples', 'oranges', 'bananas', 'grapes', 'strawberries'];

    $.each(list, function (index, value) {
        console.log( 'Item at index ' + index + ' has a value of ' + value);
    });

Output:

    Item at index 1 has a value of apples
    Item at index 2 has a value of oranges
    Item at index 3 has a value of bananas
    Item at index 4 has a value of grapes
    Item at index 5 has a value of strawberries

### $.fn.data

> Store arbitrary data associated with the matched elements.

Get

    $('#el').data('foo');

Set

    $('#el).data('foo', 'bar');

#### $.data

> Store arbitrary data associated with the specified element. Returns the value that was set.

Get

    $.data(document.body, 'foo');

Set

    $.data(document.body, 'foo', 'bar');

### $.trim

> Remove the whitespace from the beginning and end of a string.

    $.trim('      This sentence has whitespace surrounding it.       ');