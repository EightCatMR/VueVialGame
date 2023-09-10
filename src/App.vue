<script setup>
import Vial from './components/Vial.vue';
</script>

<template>
  <div style="display: grid; gap: 2rem; grid-template-columns: repeat(6, 1fr);">
    <vial v-for="(vial, index) in vials" :vial-content="vial" @click="handleSelect(index)"
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