# `rotary_encoder`
A simple rotary encoder library for the [Spade Hardware Description Language](https://spade-lang.org/).

## Usage
Due to a bug in Spade, you may not be able to import `rotary_encoder` like this:
```
use rotary_encoder::rotary_encoder;
```
Instead, use `rotary_encoder::rotary_encoder` to use it.

You will probably want to use the `Direction` enum:
```
use rotary_encoder::Direction;
```

Then create a rotary encoder:
```
let change_direction = inst rotary_encoder::rotary_encoder(clk, rst, clk_pin, dt_pin);
```
The rotary encoder needs the clk and dt inputs to be debounced. 1ms is a debounce time that works perfectly. See the [`debounce` library](https://github.com/ChocolateLoverRaj/spade_debounce/).

`change_direction` is an `Option<Direction>`. You can then process that direction. Here is an example:
```
// The rotary encoder has 60 steps to do a full rotation, so display a number from 0..60
reg(clk) position: uint<6> reset(rst: 0) = match change_direction {
    Some(Direction::Clockwise) => {
        if position == 59 {
            0
        } else {
            position +. 1
        }
    },
    Some(Direction::CounterClockwise) => {
        if position == 0 {
            59
        } else {
            position -. 1
        }
    },
    None => {
        position
    }
};
```

## Using the switch
A rotary encoder also has a "sw" input, which is the input of pressing the rotary encoder down. This library does not handle that since you can handle it just like a regular button.

## Examples
TODO

## Tests
TODO
