# Apple Pie Nav

## A Highly Customizable, easy-to-use, elegant, dropdown component

Feedback would be much appreciated, questions, suggestions, issues are more than welcome.

![MIT License](https://badgen.net/badge/license/MIT/blue 'MIT License')

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
export default {
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
