<script setup>
import Vial from './components/Vial.vue';
</script>

<template>
  <div style="display: grid; gap: 2rem; grid-template-columns: repeat(6, 1fr);">
    <vial v-for="(vial, index) in vials" :id="'vial' + String(index)" :vial-content="vial" @click="handleSelect(index)"
      :class="{ 'selected': selection == index }" />
  </div>
</template>

<script>
const colors = [
  "#451952",
  "#662549",
  "#AE445A",
  "#F39F5A",
  "#0E21A0",
  "#4D2DB7",
  "#9D44C0",
  "#EC53B0",
  "#4F709C",
  "#E5D283",
]
const newspaperSpinning = [
  { transform: "rotate(0) scale(1)" },
  { transform: "rotate(25deg) scale(1)" },
  { transform: "rotate(0deg) scale(1)" },
];

const newspaperTiming = {
  duration: 1000,
  iterations: 1,
};

export default {
  data() {
    return {
      vials: [],
      selection: null
    }
  },
  watch: {

  },
  methods: {
    getRandomInt: function (max) {
      return Math.floor(Math.random() * max);
    },
    distanceBetweenElements: function (first, second) {
      const originElement = document.querySelector(first);
      const destinationElement = document.querySelector(second);
      let originBB = originElement.getBoundingClientRect()
      let destinationBB = destinationElement.getBoundingClientRect()
      return { x: Math.round(destinationBB.x + destinationBB.width / 2 - originBB.x), y: Math.round(destinationBB.y - originBB.y) }
    },
    fillVials: function () {
      let pool = {}
      colors.forEach((element) => {
        pool[element] = 8
      })

      for (let i = 0; i < 10; i++) {
        let vial = []
        for (let j = 0; j < 8; j++) {
          let color = null
          while (!color) {
            const index = this.getRandomInt(10)
            color = colors[index]
            if (pool[color] < 1)
              color = null
            else
              pool[color] -= 1
          }
          vial.push(color)
        }
        this.vials.push(vial)
      }
      this.vials.push([])
      this.vials.push([])
    },
    handleSelect: function (index) {
      if (!this.selection) {
        this.selection = index
      } else {
        if (this.selection == index) {
          this.selection = null
          return
        }
        if (this.vials[index].length == 8)
          return
        const originTopColor = this.vials[this.selection].pop()
        const destinationTopColor = this.vials[index].pop()
        console.log(originTopColor, destinationTopColor)
        if ((originTopColor == destinationTopColor) || (destinationTopColor == null)) {
          const distance = this.distanceBetweenElements("#vial" + String(this.selection), "#vial" + String(index))
          console.log(distance)
          document.querySelector("#vial" + String(this.selection)).animate([{ transform: "rotate(0) scale(1)" },
          { transform: `rotate(15deg) scale(1) translate(${distance.x}px, ${distance.y}px)` },
          { transform: "rotate(0deg) scale(1)" }], newspaperTiming)

          if (destinationTopColor)
            this.vials[index].push(destinationTopColor)
          this.vials[index].push(originTopColor)
        } else {
          this.vials[this.selection].push(originTopColor)
          this.vials[index].push(destinationTopColor)
        }
        this.selection = null
      }
    }
  },
  mounted: function () {
    this.fillVials()
  }
}
</script>

<style scoped>
.selected {
  box-shadow: 10px 5px 5px gray;
}
</style>