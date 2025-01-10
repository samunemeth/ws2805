# ESPHome WS2805 Implementation

## TODO (for now)

  - [ ] Update the `NeoPixelBus` library, test it, then push it into esphome core.
  - [ ] Look into the exact timing required for the chips, and check out existing
  implementations for ideas.

## Challenges

  - The code is really complicated, I still need time to fully understand it.
  - The `ESPColorView` object only supports *RGBW*. This means that big changes are needed for a complete implementation.

## Backend changes required

The main branch needs to **update the `NeoPixelBus` library**.
This is required, as the current version does not support the *WS2805* chipset.

## Using `esp32_rmt_led_strip`

The implementation in current version is really hacky.
It only works when it fells like it, but it is good for a proof of concept.

By sending the last byte twice, we basically clone the first white channels
value to the second one. This however still doesn't fully work for some reason.
I suspect that either there are memory leaks/issues, or the timings are not correct.

## Using `NeoPixelBus`

First, I'm planning a "hacky" implementation. *(Be advised: It is stupid)*

When `get_view_internal` returns the `ESPColorView` object, the pointeres can be shifted,
such that the last channel is left out, and remains empty.
This means, that the *RGB* values will be in the right place, and all we have to do,
is clone the last value representing the *W* component to the empty places in the `write_state` function.

## Long term, and full implementation

Optimally, both methods should work for controlling the LEDs.
For a full implementation however, big changes to the light component
(mainly to the `ESPColorView`) to accommodate the extra channel.

The `RGBWW` components code can also help to implement the
management of white channels.