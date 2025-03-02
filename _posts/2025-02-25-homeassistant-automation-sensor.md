---
layout: post
title:  "Running automation indicator in Home Assistant"
date:   2025-02-25 22:36:00 +0100
categories: home assistant automation sensor
---

To have a better feeling of the state of the smart home installation, knowledge about the currently running automations helps. This also help detect errors earlier. Distinguishing between a light automation that fails because of a low battery, a faulty sensor or a wrong state is essential. For easiest access the state should be visible with a small indicator in the dashboard.

## Task: Display a running indicator in card

<br>

{% maincolumn 'images/hall_running_automation.gif' 'room-card with automation indicator' %}{% marginnote 'mn-hall-automation' "The state of the automation can be seen from the purple icon in the top right corner" %}

## History:
Adding running states to automations has been discussed before. {% sidenote 'automation1' '[HA Community Discussion 1](https://community.home-assistant.io/t/add-a-running-state-for-automations/479197/32)' %} {% sidenote 'automation2' '[HA Community Discussion 2](https://community.home-assistant.io/t/how-to-access-state-of-the-automation-running-not-running/460705/1)' %}
The following steps only expand on that. 

## Preparation
Have a working automation activated in home-assistant.

## Step 1 - Add a helper sensor 

1. Go to your helpers 
\<homeassistant_url>/config/helpers

2. Create new helper

3. Choose Template - Template for Sensor

4. Enter a name and input the following state code

{% raw  %}
```yaml
{{ state_attr('automation.<YOUR_AUTOMATION>', 'current') > 0 }}
```
{% endraw %}

{% raw  %}
replace \<YOUR_AUTOMATION> with the automation name e.g.
```yaml
{{ state_attr('automation.licht_hwr', 'current') > 0 }}
```
{% endraw %}
The preview should already show you if the automation is running or not. 
Save with OK

## Step 2 - Icon
Add an icon to indicate the status. I am using the room-card. {% sidenote 'room-card' '[room-card](https://github.com/marcokreeft87/room-card)' %}


```yaml
info_entities:
  - entity: automation.licht_lager
    show_icon: true
    show_name: false
    icon:
      conditions:
        - condition: equals
          entity: sensor.lager_lightautomation_running
          icon: mdi:motion-outline
          value: "False"
        - condition: equals
          entity: sensor.lager_lightautomation_running
          value: "True"
          icon: mdi:motion
          styles:
            color: mediumpurple
            animation: pulse 3.2s linear infinite
```

<br>
The full code for the card:
{% maincolumn 'images/hwr_room-card_c.png' 'HWR Room Card' %}

``` yaml
type: custom:room-card
title: HWR
icon: mdi:state-machine
show_icon: true
entity: light.wandschalter_hwr_schalter
info_entities:
  - entity: automation.licht_hwr
    show_icon: true
    show_name: false
    icon:
      conditions:
        - condition: equals
          entity: sensor.hwr_lightautomation_running
          icon: mdi:motion-outline
          value: "False"
        - condition: equals
          entity: sensor.hwr_lightautomation_running
          value: "True"
          icon: mdi:motion
          styles:
            color: mediumpurple
            animation: pulse 3.2s linear infinite
  - entity: sensor.sonoff_snzb_02d_luftfeuchtigkeit
  - entity: sensor.sonoff_snzb_02d_temperatur
entities:
  - entity: binary_sensor.waschmaschine
    name: Waschmaschine
    show_icon: true
    show_name: false
    show_state: false
    icon:
      state_on: mdi:washing-machine
      state_off: mdi:washing-machine-off
      template:
        styles: |
          if (entity.state == 'on') return 'animation: shake 1.4s infinite;';
  - entity: light.wandschalter_hwr_schalter
    name: Light Automation
    show_icon: true
    show_name: false
    show_state: false
card_mod:
  style: |
    .entity {
      top: -0px;
      left: -2px;
      position: relative;
    }
    .entities-row .icon-small {
      top: -2px;
      left: -2px;
    }
    .entities-info-row .icon-small {
      top: -2px;
    }
    .entities-info-row {
      margin-right: -20px;
    }
    .entities-info-row .icon-small {
      top: -px;
    }
    .main-state {
      margin-top: -px;
      width: 32px;
    }
        @keyframes shake {
      0% { transform: translate(1px, 1px) rotate(0deg); }
      10% { transform: translate(-1px, -2px) rotate(-10deg); }
      20% { transform: translate(-3px, 0px) rotate(1deg); }
      30% { transform: translate(3px, 2px) rotate(0deg); }
      40% { transform: translate(1px, -1px) rotate(1deg); }
      50% { transform: translate(-1px, 2px) rotate(-1deg); }
      60% { transform: translate(-3px, 1px) rotate(0deg); }
      70% { transform: translate(3px, 1px) rotate(-1deg); }
      80% { transform: translate(-1px, -1px) rotate(10deg); }
      90% { transform: translate(1px, 2px) rotate(0deg); }
      100% { transform: translate(1px, -2px) rotate(-1deg); }
    }
```