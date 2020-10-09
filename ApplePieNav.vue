<template>
  <div v-show="visible" class="applePie">
    <svg :width="square" :height="square">
      <defs>
        <!-- VISUAL MASK -->
        <mask :id="`mask_${_uid}`">
          <polygon :points="ZONE" fill="#ffffff" />
          <circle
            :cx="marginMouseX"
            :cy="marginMouseY"
            :r="mask"
            fill="#000000"
          />
        </mask>
      </defs>

      <!-- ZONE PIES -->
      <polygon
        v-for="(zone, index) in zones"
        :key="`zone_${index}`"
        :transform="`rotate(${
          index * zoneDegrees
        },${marginMouseX},${marginMouseY})`"
        :points="ZONE"
        class="zone"
        :mask="`url(#mask_${_uid})`"
        @mouseover="hoverZone(index)"
        @mouseout="leaveZone"
        @click="activateZone(index)"
      />
      <foreignObject
        v-for="(zone, index) in zones"
        :key="`zonef_${index}`"
        :x="marginMouseX - 1"
        :y="marginMouseY - (mask + buffer)"
        width="2"
        height="2"
        :transform="`rotate(${
          index * zoneDegrees
        },${marginMouseX},${marginMouseY}) rotate(${
          -1 * index * zoneDegrees
        },${marginMouseX},${marginMouseY - (mask + buffer) + 1})`"
        class="pointer-events-none overflow-visible"
      >
        <div
          class="flex flex-row w-full h-full justify-center items-center overflow-visible text-white font-black"
        >
          <h1 class="text-center text-3xl">{{ zone.label }}</h1>
        </div>
      </foreignObject>

      <!-- INTERACTION MASK -->
      <circle
        :cx="marginMouseX"
        :cy="marginMouseY"
        :r="mask"
        fill="transparent"
      />
    </svg>
  </div>
</template>

<script>
import VueMousetrap from 'vue-mousetrap'
Vue.use(VueMousetrap)

export default {
  props: {
    trigger: { type: String, default: () => 'space' },
    zones: { type: Array, default: () => [] },
    mask: { type: Number, default: () => 70 },
    buffer: { type: Number, default: () => 150 },
    margin: { type: Number, default: () => 300 },
  },
  data() {
    return {
      dummyZone: { label: 'Undefined Zone', exec: () => {} },
      hoveredZone: null,
      keyDown: false,
      winW: 0,
      winH: 0,
      mouseX: 0,
      mouseY: 0,
      mouseX2: 0,
      mouseY2: 0,
    }
  },
  computed: {
    zoneDegrees() {
      return 360 / this.zones.length
    },
    visible() {
      return this.keyDown
    },
    square() {
      return Math.max(this.winW, this.winH)
    },
    center() {
      return `${this.marginMouseX},${this.marginMouseY}`
    },
    marginMouseX() {
      return this.mouseX > this.winW - this.margin
        ? this.winW - this.margin
        : Math.max(this.margin, this.mouseX)
    },
    marginMouseY() {
      return this.mouseY > this.winH - this.margin
        ? this.winH - this.margin
        : Math.max(this.margin, this.mouseY)
    },
    /**
     * DYNAMIC ZONE
     */
    tanLength() {
      return this.square * Math.tan(((this.zoneDegrees / 2) * Math.PI) / 180)
    },
    zoneLeft() {
      return `${this.marginMouseX - this.tanLength}, ${
        this.marginMouseY - this.square
      }`
    },
    zoneRight() {
      return `${this.marginMouseX + this.tanLength}, ${
        this.marginMouseY - this.square
      }`
    },
    ZONE() {
      return `${this.center} ${this.zoneLeft} ${this.zoneRight}`
    },
  },
  created() {
    addEventListener('resize', this.handleResize)
    addEventListener('mousemove', this.handleMousePos)
    this.limitZones()

    this.handleResize()

    this.$mousetrap.bind(this.trigger, (e) => {
      if (!this.keyDown) {
        this.keyDown = true
        this.hoverZone(null)
      }
    })
    this.$mousetrap.bind(
      this.trigger,
      () => {
        this.keyDown = false
        this.mouseX = this.mouseX2
        this.mouseY = this.mouseY2
        this.activateZone(this.hoveredZone)
      },
      'keyup'
    )
  },
  destroyed() {
    removeEventListener('resize', this.handleResize)
    removeEventListener('mousemove', this.handleMousePos)
  },
  methods: {
    limitZones() {
      if (this.zones.length > 9) {
        this.zones.splice(8)
      } else if (this.zones.length < 3) {
        const fillZones = Array(3 - this.zones.length).fill(this.dummyZone)
        this.zones.push(...fillZones)
      } else {
      }
    },
    hoverZone(zone) {
      this.hoveredZone = zone
    },
    leaveZone() {
      this.hoveredZone = null
    },
    activateZone(zone) {
      // Only execute if the active zone is not null, meaning it's still hovered.
      // This prevents a zone from activating if the user mouses out of the entire page.
      if (zone !== null) {
        this.zones[zone].exec()
      }
    },
    handleResize() {
      this.winW = window.innerWidth
      this.winH = window.innerHeight
    },
    handleMousePos(e) {
      if (this.keyDown) {
        this.mouseX2 = e.clientX
        this.mouseY2 = e.clientY
        return
      }
      this.mouseX = e.clientX
      this.mouseY = e.clientY
    },
  },
}
</script>

<style lang="scss">
.applePie {
  position: absolute;
  z-index: 1000;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
}
.zone {
  fill: theme('colors.blue.700');
  opacity: 0.75;
  &:hover {
    fill: theme('colors.gray.900');
    opacity: 0.85;
  }
}
</style>
