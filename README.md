<img src="icon.png" width="96" align="right" alt="">

# Energy Pebble for Home Assistant

Shows your [Energy Pebble](https://energypebble.tdlx.nl)'s colour signal in Home
Assistant: a sensor holding the current colour (`green` / `yellow` / `red`) with
the next 8 hours as attributes.

The data is personalised to your household profile, because the integration
polls the same endpoint your physical pebble does. A fixed-price contract, solar
panels or a home battery change what the pebble shows, and the sensor follows.

## Install

### HACS

1. HACS, then the three-dot menu, then **Custom repositories**.
2. Add `https://github.com/thomaskgb/energy-pebble-homeassistant`, category
   **Integration**.
3. Install **Energy Pebble**, then restart Home Assistant.

### Manually

Download the latest release, unzip it next to your `configuration.yaml`, and
restart. The archive already contains `custom_components/energy_pebble/`.

## Set up

1. On [energypebble.tdlx.nl](https://energypebble.tdlx.nl/dashboard), open your
   user menu, then **Settings, Account**, and create a token. It is shown once,
   so copy it then. A token acts as you and never as an admin.
2. In Home Assistant: **Settings, Devices & services, Add integration, Energy
   Pebble**. Paste the token and pick which pebble to follow.

## Entity

`sensor.<nickname>_color` holds the current colour, an enum of `green`,
`yellow` and `red`, with these attributes:

| attribute | meaning |
| --- | --- |
| `next_hours` | hour and colour for the next 8 hours |
| `signal_source` | which rule produced the colour: price, solar, day/night or fixed |
| `personalized` | whether a household profile was applied |
| `display` | palette, brightness and night dimming, as the device sees them |

## Example automation

```yaml
automation:
  - alias: Start dishwasher when the pebble turns green
    triggers:
      - trigger: state
        entity_id: sensor.kitchen_color
        to: "green"
    actions:
      - action: switch.turn_on
        target:
          entity_id: switch.dishwasher
```

## Licence

MIT. See [LICENSE](LICENSE).

## Where the code lives

This repository is the home of the integration, because HACS resolves
`custom_components/<domain>/` from a repository root. The service it talks to
lives in [energy-pebble-api](https://github.com/thomaskgb/energy-pebble-api).
