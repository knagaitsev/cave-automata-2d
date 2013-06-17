# cave-automata-2d #

Generate 2D cave layouts in JavaScript: an implementation of
the first half of
[this paper](http://julian.togelius.com/Johnson2010Cellular.pdf) using
[ndarrays](http://github.com/mikolalysenko/ndarray).

## Installation ##

``` bash
npm install cave-automata-2d
```

## Usage ##

### `iterate = cave(ndarray[, opts])` ###

Takes an ndarray and prepares it for cave generation. Options include:

* `density`: The probability of each filled cell being set to `1` ("on"),
  as opposed to `0` ("off"). Defaults to 0.5.
* `threshold`: The minimum surrounding cells required for a cell to survive an
  iteration. Defaults to 5.
* `hood`: The maximum distance of neighboring cells to check. Defaults to 1.
* `fill`: By default, the ndarray is populated with random values. If you need
  to disable this, pass `fill: false`.
* `iterations`: The amount of iterations to apply immediately. Defaults to 0.

Note that this function will modify the array directly.

### `iterate([iterations])` ###

The above function will return an "iterate" function, which will step the
simulation forward a specific number of ticks. This will generate the cave
layout - the best amount of iterations to run varies depending on the
parameters above.

Because of the way the module works under the hood, there's less overhead by
iterating an even number of times.

## Example ##

``` javascript
var cave = require('cave-automata-2d')
  , ndarray = require('ndarray')
  , width = 50
  , height = 50

var grid = ndarray.zeros([ width, height ])

// Fill the grid with random points,
// returning an "iterate" method.
var iterate = cave(grid, {
    density: 0.5
  , threshold: 5
  , hood: 1
  , fill: true
})

// Iterate the grid five times to generate
// a smooth-ish layout.
iterate(5)
```

You can see the generator at work by running `eyeball-test.js` in Node, which will spit out something similar to this:

```
 @@@@@@@@@@@@@@@        @@@@@@               @@@@@@@@         @@@@@@@@@@@@      @@@@@@
  @@@@@@@@@@@@@@          @@@                    @@@@@@@       @@@@@@@@@@@       @@@
    @@@@@@@@@@@            @@             @@      @@@@@@@       @@@@@@@@@
     @@@@@@@@@             @@@           @@@@      @@@@@@       @@@@@@@@@
      @@@@@@@             @@@@@         @@@@@@     @@@@@@      @@@@@@@@@@            @@
       @@@@@@             @@@@@        @@@@@@@     @@@@@@@    @@@@@@@@@@@          @@@@@
        @@@@@@             @@@         @@@@@@@     @@@@@@@@   @@@@@@@@@@@         @@@@@@
        @@@@@@                          @@@@@@     @@@@@@@@  @@@@@@@@@@@      @@@@@@@@@@
        @@@@@@                           @@@@@@     @@@@@@   @@@@@           @@@@@@@@@@
        @@@@@                            @@@@@@@      @@@    @@@@           @@@@@@@@@@@
       @@@@@@                            @@@@@@@@             @@@          @@@@@@@@@@@
       @@@@@                       @@   @@@@@@@@@@            @@@@         @@@@@@@@@@
  @@  @@@@@@               @@@    @@@@@@@@@@@@@@@@@          @@@@@@       @@@@@@@@@@@
 @@@@@@@@@@@              @@@@@   @@@@@@@@@@@@@@@@@@      @@@@@@@@@       @@@@@@@@@@
 @@@@@@@@@@               @@@@@    @@@@@@@@@@@@@@@@@@    @@@@@@@@@@       @@@@@@@@@@
  @@@@@                    @@@      @@@@@@@@@@@@@@@@@   @@@@@@@@@@@@      @@@@@@@@@
                                    @@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@   @@@@@@@@@
                                   @@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@@@@
                                   @@@@@@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@@@@
                                    @@@@@@@@@@@@@@@@   @@@@@@@@@@@@@@@@@@@@@@@@@@
                                      @@@@@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@@@
           @@@                           @@@@@@@@@@    @@@@@@@@@@@@@@@@@@@@@
          @@@@@@@@                        @@@@  @@@@    @@@@@@@@@@@@@@@@@@
         @@@@@@@@@@@                      @@@    @@@@@     @@@@@@@@@@@@@@
         @@@@@@@@@@@@@@@@@               @@@@    @@@@@@     @@@@@@@@                  @@
  @@@   @@@@@@@@@@@@@@@@@@@@@@           @@@@    @@@@@@     @@@@@@@                  @@@@
 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@          @@@@@  @@@@@@     @@@@@@@@            @@   @@@@@
 @@@@@@@@@@@@@@@@@@  @@@@@@@@@@@         @@@@@@@@@@@      @@@@@@@@@@          @@@@@@@@@@
 @@@@@@@@@@@@@@@      @@@@@@@@@@         @@@@@@@@@@       @@@@@@@@@@@         @@@@@@@@@
@@@@@@@@@@@@@@@       @@@@@@@@@           @@@@@@@@@        @@@@@@@@@@@@@       @@@@@@@
 @@@@@@@@@@@@@@@        @@@@@@               @@@@@@@@         @@@@@@@@@@@@      @@@@@@
```

Neat, huh?
