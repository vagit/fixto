# fixto

A jQuery plugin for sticky positioning. Fix containers to the viewport relative to an ancestor. To see it in action you can see the [demo page][demo] or development pages [development page 1][dev1], [development page 2][dev2], [development page 3][dev3].

[demo]: http://bbarakaci.github.com/fixto
[dev1]: http://bbarakaci.github.com/fixto/dev1.html
[dev2]: http://bbarakaci.github.com/fixto/dev2.html
[dev3]: http://bbarakaci.github.com/fixto/dev3.html

- [Features](#features)
- [Browser support](#browser-support)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Styling](#styling)
- [Options](#options)
- [Public Methods](#public-methods)
- [Known issues](#known-issues)
- [Release notes](#release-notes)

## Features
- Responsive
- Handles multiple instances
- Start, stop, destroy
- Sensitive to viewport height
- Handles positioning context created by transformed ancestors.
- Uses native position sticky when available

## Browser support

Modern browsers, ie8+ are supported.

## Getting Started
Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/bbarakaci/fixto/master/dist/fixto.min.js
[max]: https://raw.github.com/bbarakaci/fixto/master/dist/fixto.js

Add jQuery

    <script src="jquery.js"></script>
    
Add fixTo

    <script src="fixto.min.js"></script>

## Usage

Use it with jQuery

    $('#nav').fixTo('body');
    
You can fix multiple containers to multiple ancestors. Make sure your selectors match exactly.

    $('.sticky').fixTo('.sticky-holder');
    
Passing options

    $('#left-banner').fixTo('#left-column', {
        className : 'my-class-name',
        zIndex: 10,
        mind: '#header',
        top: 20
    });
    
Instantiate without jQuery:
    
    var sticky = fixto.fixTo(domElementToFix, domElementToBeFixed, options);

## Styling

When the container is fixed, it will receive the class name `fixto-fixed`. You may use this class or you may pass any other class name as an option.

## Options

### className (String)

See above example.

### zIndex (Number)

Although you can set z-index with css, it is possible to pass a `zIndex` option. See above example.

### mind (selector)

When you have other fixed containers on a page, pass those containers as mind option to prevent overlapping.

Example

    $('#header').fixTo('body');

    $('#left-banner').fixTo('#left-column', {
        mind: '#header'
    });
    
Selector can be in any form that jQuery can handle. You can pass multiple elements.

### top (Number)

In pixels. Fixed element will preserve a gap on its top.

### useNativeSticky (Boolean)
Fixto will use native position sticky when supported by the browser. Set this option to `false` if you want to disable native position sticky.

This option can not be overwritten after initialization. If you need to do so, you can destroy the instance and create a new one.

While fixto can fix a container to any of its ancestors, native sticky will fix it to the nearest containing block, which is generally its parent. 

There is no native way to know if the container is sticked, so it will not receive the passed class name. 

Native sticky will perform very well as all the work is done by the browser. Without native sticky you will notice delayed response on IOS as all the javascript execuion is halted on scroll.

## Public Methods

Following methods can be called directly on the instance or with jQuery.

### refresh

Fixto tries to adapt to the layout during scroll and window resize. In case the layout changes dynamically at any time, use refresh to force fixto to adapt. 

    instance.refresh();
    
jQuery:
    
    $('#nav').fixTo('refresh');

### setOptions(options)

Resets the options. Fixto will refresh itself automatically after this method is used.

    instance.setOptions({
        top: 10
    });
    
jQuery:
    
    $('#nav').fixTo('setOptions', {
        top: 10
    });

### destroy

Destroys the instance:

    instance.destroy();
    
jQuery:
    
    $('#nav').fixTo('destroy');
    
### stop

Stops the instances behavior without destroying the instance.

    instance.stop();

jQuery:

    $('#nav').fixTo('stop');
    
### start

Starts the instances behavior.

    instance.start();

jQuery:

    $('#nav').fixTo('start');

## Known issues

- Doesn't work on elements having `margin:auto`. You will need an additional wrapper around the element. This is because webkit differs from other browsers about reporting the computed margin values.
- There is flickering on Safari when there is a transformed parent and if native sticky positioning is not used. Still couldn't resolve that. Safari already supports native sticky positioning so you will observe this issue only if you disable native sticky support.

## Release notes
### 0.3.0
- Use native position sticky when supported.
- Margin top of the fixed element will be ignored at fixed state, to be consistent with native sticky. This might break your existing layout if you used margin top on the target container with previous versions of fixto.
- Top option added
- useNativeSticky option added
- setOptions method added
- refresh method added
- No need to use it on document ready. Feature detection will be done during first instantiation.

### 0.2.0
- Ancestors with css transform rules does not break the functionality anymore.
- Needs to be used only during or after document ready
