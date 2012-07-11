# jQuery - Part 2

## Topics

* Performance Optimization
* AJAX
* Event delegation
* Animations
* Plugins
* Utility functions

## Performance Optimization

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

### What is AJAX?

AJAX stands for **A**synchronous **J**avaScript **a**nd **X**ML. However, using AJAX doesn't necessarily mean using XML. It's the *asynchronous* part that's significant. To understand why, let's step back a little bit and look at how websites and web applications work without AJAX. 

#### Without AJAX

When, for example, you click a link or submit a form, the browser makes a request to the server *synchronously*. What that means, in practical terms, is that the whole web page is refreshed with new content from the server. Say you have a contact form and the server is set to send a notification email when the form is submitted. Without AJAX, the user has to wait until the email is sent and the page refreshes before they see a response from the server.

#### With AJAX

Using AJAX, when you click a link or submit a form, JavaScript sends a request to the server *asynchronously*, and receives (usually only pertinent) data back. When submitting the aforementioned form, the form data will be sent through AJAX and the server will handle sending the email in the background. The user can do other things on the page while the server is handling the request. When it's finished it can send back a response that AJAX will catch and perhaps let the user know their message was sent successfully or not.

Since the whole page doesn't refresh, actions such as submitting a form take less time and less bandwidth. The server only sends back a response with, perhaps, some requested data instead of the whole HTML page that the browser has to re-parse.

AJAX is especially useful in web applications, where you can incorporate a myriad of interactions with it. Gmail, for example, is one of the first and most notable web apps that makes heavy use of AJAX. 

Most websites use AJAX sparingly, for, say submitting a form or something similar. Navigating between pages is done traditionally with a round trip to the server. However, there has been a trend lately towards using AJAX more heavily, handling a lot of the work in the browser that used to be left to the server. This is most common with web applications, but some websites do this as well.

AJAX can be used with servers other than your own. You can utilize a service's API to get data and use it on your own site. Below, we'll see an example of that.

### Using AJAX with vanilla JavaScript

AJAX is provided by the browser through the XMLHttpRequest object. However, due to inconsistencies between browsers, it can be quite a pain to use. For now, let's focus on jQuery's AJAX methods.

### Using AJAX with jQuery

jQuery comes to the rescue again, ironing out browser differences and giving us a nice, concise API for using AJAX.

The primary method for using is `$.ajax`. The `$.ajax` methods takes an configuration object that can take numerous options. The following are some of the most common ones to use:

`url` : The url (usually a server endpoint) where you wish to send the request.

`type` : The type of request, usually 'POST' or 'GET.' Others, such as 'PUT' or 'DELETE,' can be used, but they are not supported by all browsers.

`dataType` : The type of data you expect back from the server, such as:

* xml
* html
* script
* json
* jsonp

If this isn't specified, jQuery will do its best to guess what the type is when the data is returned.

`data` : Data sent to the server. Usually an object of key/value pairs.

`success` : A callback run if the request was successful.

`failure` : A callback run if the request failed.

#### Examples

Submit a form

	// Cache the form, since it's used multiple times below
	var $myForm = $('#my-form');
	
	$myForm.submit(function (e) {
		// prevent the form from being submitted synchronously
		e.preventDefault();
		
		// submit the form through ajax
		$.ajax({
			url : '/handle-form.php',
			type : 'post',
			data :  $myForm.serialize(),
			success : function (data, textStatus, jqXHR) {
				 $myForm.slideUp('fast', function () {
					$('body').append('Your message was sent successfully');
				});
			},
			failure : function () {
				 $myForm.before('Your message could not be sent. Please try again');
			}
		});
		
	});

A number of sites provide APIs so you can access their data and use it on your site. Some examples of APIs I've used with AJAX are YouTube, Twitter, and Rotten Tomatoes. 

Often, to use an API, you need to register and obtain an API key. You also need to respect certain usage quotas. Don't make thousands of requests in a minute. 

Here is an example using the YouTube API to get a list of the most popular videos:

	$.ajax({
	    dataType : 'jsonp',
	    url : 'http://gdata.youtube.com/feeds/api/standardfeeds/most_popular?v=2&alt=json',
	    success : function (data) {
	        // This is an array of the videos
	        var videos = data.feed.entry;
	
	        // Loop through the videos and log their title
	        $.each(videos, function () {
	            console.log( this.title.$t );
	        });
	    }
	});

### Convenience methods

jQuery provides a number of convenience methods for common AJAX actions. Internally, they all call `$.ajax` with some set configurations. I recommend checking out the documentation for them on the [jQuery site](api.jquery.com).

Here are a few of them:

* `$.get`
* `$.post`
* `$.getJSON`
* `$.load`
* `$.fn.load`

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
$.fn.stop

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