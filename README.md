# Apple Pie Nav

## A hyper-quick pie-menu method action system.

Feedback would be much appreciated, questions, suggestions, issues are more than welcome.

![MIT License](https://badgen.net/badge/license/MIT/blue 'MIT License')

#### How it works

When implemented, this navigation component lets you press a hotkey (default: spacebar) which immediately brings up a pie-like navigation component. It tries to center around the mouse center, but adjusts given certain rules so that all options remain visible. While the hotkey is depressed, user can simply move the mouse over the desired zone and release the hotkey. The associated method (typically navigation, but could be anything) will then fire, and the component will hide.

###### Demo

![A usage demo gif](https://media.giphy.com/media/Ir7M8IlvsXmx5HkpTv/giphy.gif)

# Usage

In your own Vue scripts:

```HTML
<template>
  <div>
    <ApplePie trigger="s" :zones="zones" />
  </div>
</template>

<script>
import ApplePie from 'ApplePieNav.vue'
export default {
  components: [ApplePie],
  data() {
    return {
      zones: [
        {
          label: 'Pizza',
          exec: () => {
            console.log('This is a direct method')
          }
        },
        {
          label: 'Beer',
          exec: this.sayBeer
        }
      ]
    }
  },
  methods: {
    sayBeer() {
      console.log('To drink or not to drink...')
    }
  }
}
</script>
```

# Options

Vue component parameters:
| Parameter | Purpose | Default |
| --- | --- | --- |
| **trigger** | <a href="https://github.com/ccampbell/mousetrap">Mousetrap</a> key string to activate the navigation pie. | 'space' |
| **zones** | Array of zone options. This expects between 3 and 9 zones. If fewer or more are given, they're spread or trimmed. You'll see. | Array(3).fill({ label: 'Undefined Zone', exec: () => {} }) |
| **mask** | Integer radius in pixels of the central zone mask circle where actions are ignored. | 70 |
| **buffer** | How much space to put between the edge of the mask and the zone labels in pixels. | 150 |
| **margin** | How much of a margin to leave around the inside of the main window so the pie doesn't render too close to the outside which would hide some options. | 300 |

# Features

- Prevents the pie from starting too close to the window's margin if the mouse is within said margin. This is good because it prevents options from being hidden.
- Automatically limits the number of options passed in to between 3 and 9 because, really... What do you need a pie nav for fewer or more at one time?
- Allows as many different pie navs to be loaded as you wish. Just associate them with different trigger keys and you can have multiple hyper menus available in your system.

# Dependencies

### <a href="https://github.com/g33kio/vue-mousetrap" target="_blank">Mousetrap</a>

To capture and react to user keypresses

### <a href="https://tailwindcss.com/" target="_blank">Tailwind CSS</a>

To make things look nice in a way that makes sense.

# Inspiration

### <a href="https://www.blender.org/" target="_blank">Blender</a>

I started building 3D scenes with Blender and had my mind blown by their implementation of rapid navigation with their **pie menus**. I honestly believe it's one of the fastest ways to navigate to different pages in a user interface because it allows you to simply gesture with your trackpad or mouse which is far simpler than hunting down a small-target menu item to get to your destination.
