# HaCasa Button Card styles

Reusable [Button Card](https://github.com/custom-cards/button-card) templates
from [Damian Eickhoff's HaCasa v2 legacy branch](https://github.com/damianeickhoff/HaCasa/tree/legacy),
imported at revision `1d1da03fe9298b96348894c74a019bcbb2177b91`.
This is a manual-install collection, not HaCasa Nova or a complete dashboard.
The upstream templates are no longer maintained.

## Contents

- **17 YAML files / 20 named templates:** the 15 card files plus shared base
  and badge templates. Includes climate, fan, glance, graph, header, light,
  media, navigation, navigation bar, scene, security, sensor, switch, title
  and weather cards, and their named variants.
- **One card-support theme:** card colors, typography, sliders and light/dark
  variables only; no kiosk mode, header hiding, view layout or popup theme.
- **Eight custom icons, 12 weather SVGs and one idle-media GIF:** only assets
  referenced by the retained templates.
- This README and the upstream MIT license. No Nova code, bundle, build tools,
  dashboard/views, example entities, screenshots, site or HACS manifest.

Templates retain upstream behavior, except `fapro:` icon references use
`local:` for the current Custom Icons integration. The support theme is trimmed
from the upstream Peach theme, with definitions for otherwise missing card
variables. It does not install or replace your dashboard.

## Install

1. Install [Button Card](https://github.com/custom-cards/button-card) through
   HACS and ensure its JavaScript resource is loaded.
2. Install the resources used by the templates you select:

   | Resource | Templates |
   | --- | --- |
   | [My Cards](https://github.com/AnthonMS/my-cards), providing `custom:my-slider-v2` | Light and fan cards |
   | [Mini Graph Card](https://github.com/kalkih/mini-graph-card) | Graph, climate and switch cards |
   | [Card Mod](https://github.com/thomasloven/lovelace-card-mod) | Graph, climate, switch and navigation bar cards |
   | [Custom Icons](https://github.com/thomasloven/hass-custom_icons) integration | Default light, fan and security icons |

   Hidden nested cards can still need their resources. Install the listed
   resource even if a slider or graph is visually disabled, or remove that
   nested card from your local template copy.
3. Copy `HaCasa/dashboard/HaCasa/templates/` to
   `/config/dashboard/HaCasa/templates/`.
4. Copy `HaCasa/www/images/` to `/config/www/images/`, preserving paths.
5. Copy the eight files in `HaCasa/custom_icons/` into
   `/config/custom_icons/`. In Custom Icons, enable **Local**, reload the icon
   collection and refresh your browser.
6. Copy `HaCasa/themes/hacasa-card-styles.yaml` to
   `/config/themes/hacasa-card-styles.yaml`. Merge this into your existing
   `configuration.yaml` (do not duplicate `frontend:`):

   ```yaml
   frontend:
     themes: !include_dir_merge_named themes
   ```

   Reload themes or restart Home Assistant and select **HaCasa Card Styles**
   in your user profile.

## Use in your existing dashboard

In a **YAML-mode dashboard**, add this at the root of its YAML file, alongside
your existing `views:`. Home Assistant's directory include loads subdirectories,
so the shared base templates are included too:

```yaml
button_card_templates: !include_dir_merge_named /config/dashboard/HaCasa/templates/
```

Then add a card to any existing view, replacing the entity with one of yours:

```yaml
type: custom:button-card
template: hc_light_card
entity: light.living_room
name: Living room
tap_action:
  action: toggle
variables:
  enable_slider: false
```

For a **storage/UI-mode dashboard**, `!include` directives are not supported.
Merge the contents of all 17 template YAML files into a single
`button_card_templates:` mapping in that dashboard's raw configuration editor.
Do not paste the filenames or add a second `button_card_templates:` key.

Each named template's `variables:` mapping describes its options. Supply your
own entities, navigation paths and actions. In particular, configure glance
entities, scene/input-select options and navigation bar items before using
those cards. Override the title back button's inherited `/home` navigation
path for your dashboard. Do not store alarm codes in shared/public YAML.

## Legacy limitations

These are extracted legacy templates, not a compatibility rewrite. Static
YAML, JavaScript syntax and reference checks are not a Home Assistant runtime
test. Some cards assume entity-specific attributes.

- The weather forecast reads the old `attributes.forecast` array (at least
  four entries). Modern weather entities generally do not expose it.
  Disable it with `variables: {show_forecast: false}`; if your Button Card
  version still evaluates hidden nested cards, remove `custom_fields.f`
  from your local weather template or provide a compatible forecast entity.
- The supplied weather art covers only `clear-night`, `cloudy`, `partlycloudy`,
  `rainy`, `snowy` and `sunny`. Other conditions generate filenames not present
  upstream; supply corresponding art or customize the weather/header templates
  if you need those conditions.
- Colors use the original `ha-card-backgound-active` spelling intentionally,
  because the scene template references it.

## Attribution and license

HaCasa templates and assets were created by
[Damian Eickhoff](https://github.com/damianeickhoff) and the
[HaCasa contributors](https://github.com/damianeickhoff/HaCasa/graphs/contributors).
Button Card is maintained by the custom-cards project.
The original MIT copyright and permission notice are preserved in [LICENSE](LICENSE).
