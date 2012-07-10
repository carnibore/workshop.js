# jQuery - Part 2

## Topics

* Performance
* AJAX
* Event delegation
* Animations
* Plugins
* Utility functions

## Performance

cache selectors

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