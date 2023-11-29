<script>
export default {
  props: {
    rate: {
      type: [String, Number],
      default: 0,
    },
    index: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      innerRing: [],
      outerRing: [],
      paths: [],
    }
  },
  mounted() {
    this.outerRing = this.setPoints(42)
    this.innerRing = this.setPoints(32)
    this.outerRing.forEach((point, index) => {
      const point1 = this.innerRing[index]
      const d = `M${point[0].x} ${point[0].y} L${point[1].x} ${point[1].y} L${point1[1].x} ${point1[1].y} L${point1[0].x} ${point1[0].y}`
      this.paths.push(d)
    })
  },
  methods: {
    setPoints(r) {
      const points = []
      let angle = -90
      for (let i = 0; i < 40; i++) {
        const radian1 = Math.PI / 180 * angle
        const radian2 = Math.PI / 180 * (angle + 7)
        points.push([
          {
            x: Math.cos(radian1) * r + 50,
            y: Math.sin(radian1) * r + 50,
          },
          {
            x: Math.cos(radian2) * r + 50,
            y: Math.sin(radian2) * r + 50,
          },
        ])
        angle += 9
      }
      return points
    },
  },
}
</script>

<template>
  <svg viewBox="0 0 100 100" version="1.1" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <linearGradient id="gradient0" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" style="stop-color: #2DF7FF;" />
        <stop offset="100%" style="stop-color: #58FF83;" />
      </linearGradient>
      <linearGradient id="gradient1" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" style="stop-color: #66FFB0;" />
        <stop offset="100%" style="stop-color: #FFA800;" />
      </linearGradient>
    </defs>
    <g fill="rgba(0,134,255,0.6)">
      <path :d="paths.join(' ')" />
    </g>
    <g :fill="`url(#gradient${index})`">
      <path :d="paths.slice(0, Math.ceil(40 * rate / 100)).join(' ')" />
    </g>
  </svg>
</template>
