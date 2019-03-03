<template lang="pug">
  .home
    input(v-model="offset")
    br
    span(v-for="(v, n) in names" :key="v.name" v-html="v.svg") 
    br
    button(@click="addMore") More
</template>

<script>
import { pour, PlainSVGStringRenderer } from 'sigil-js'
import dirs from '../assets/dirs.json'
export default {
  name: 'home',
  data() {
    return {
      offset: 0,
      names: [],
      additional: 10
    }
  },
  methods: {
    addMore () {
      for (let i = (this.offset * this.additional); i < ((this.offset + 1) * this.additional); i++) {
        let name = this.getNameFromN(i)
        const r = pour({
          patp: name,
          renderer: undefined,
          size: 128
        });
        const data = PlainSVGStringRenderer.svg(r)
        this.names.push({
          name,
          svg: data
        })
      }
      this.offset += 1
    },
    getNameFromDirs({NW, NE, SW, SE}) {
      return dirs.NW[NW] +
        dirs.NE[NE] +
        dirs.SW[SW] +
        dirs.SE[SE]
    },
    getNameFromN (n) {
      let NW = n % dirs.NW.length
      n =  Math.floor(n / dirs.NW.length)
      let NE = n % dirs.NE.length
      n =  Math.floor(n / dirs.NE.length)
      let SW = n % dirs.SW.length
      n =  Math.floor(n / dirs.SW.length)
      let SE = n % dirs.SE.length
      return this.getNameFromDirs({NW, NE, SW, SE})
    }
  }
}
</script>
