<template lang="pug">
  .home
    .pic
      .bigboy(:style="`background-image: url(data:image/svg+xml;base64,${svg});`")
      //- .bigboy(v-html="svg")
      .NW.dir(@click="clickDir('NW')")
      .NE.dir(@click="clickDir('NE')")
      .SW.dir(@click="clickDir('SW')")
      .SE.dir(@click="clickDir('SE')")
    table
      tr
        th(colspan="2") {{(prettyName)}}
      tr
        th NW
        th NE
      tr
        td 
          input(type="number" v-model="NW")
        td
          input(type="number" v-model="NE")
      tr
        th SW
        th SE
      tr
        td
          input(type="number" v-model="SW")
        td
          input(type="number" v-model="SE")
    button(@click="randomize") RaNdOm
</template>

<script>

import { pour, PlainSVGStringRenderer } from 'sigil-js'
import dirs from '../assets/dirs.json'
export default {
  name: 'home',
  data() {
    return {
      NW: rand(dirs.NW.length),
      NE: rand(dirs.NE.length),
      SW: rand(dirs.SW.length),
      SE: rand(dirs.SE.length)
    }
  },
  computed: {
    prettyName() {
      return '~' + JSON.parse(JSON.stringify(this.name)).splice(6, 0, '-')
    },
    name () {
      return this.getNameFromDirs({NW: this.NW, NE: this.NE, SW: this.SW, SE: this.SE})
    },
    svg () {
      const r = pour({
        patp: this.name,
        renderer: undefined,
        size: 128
      });
      return btoa(PlainSVGStringRenderer.svg(r))//.replace(/\r/g, "").replace(/\n/g, "");

    }
  },
  methods: {
    randomize () {
      this.NW = rand(dirs.NW.length)
      this.NE = rand(dirs.NE.length)
      this.SW = rand(dirs.SW.length)
      this.SE = rand(dirs.SE.length)
    },
    clickDir (dir) {
      switch (dir){
        case 'NW':
          this.NW = (this.NW + 1) % dirs.NW.length
        break;
        case 'NE':
          this.NE = (this.NE + 1) % dirs.NE.length
        break;
        case 'SW':
          this.SW = (this.SW + 1) % dirs.SW.length
        break;
        case 'SE':
          this.SE = (this.SE + 1) % dirs.SE.length
        break;
      }
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
function rand(max) {
  return Math.floor(Math.random() * max);
}
if (!String.prototype.splice) {
    /**
     * {JSDoc}
     *
     * The splice() method changes the content of a string by removing a range of
     * characters and/or adding new characters.
     *
     * @this {String}
     * @param {number} start Index at which to start changing the string.
     * @param {number} delCount An integer indicating the number of old chars to remove.
     * @param {string} newSubStr The String that is spliced in.
     * @return {string} A new string with the spliced substring.
     */
    String.prototype.splice = function(start, delCount, newSubStr) {
        return this.slice(0, start) + newSubStr + this.slice(start + Math.abs(delCount));
    };
}
</script>
<style lang="stylus">
  table
    margin 10px auto 20px auto
    th
      padding-top 20px
    td
      padding 0px 20px
  .pic
    position relative
    // border 1px solid blue
    .bigboy
      width 60vh
      height 60vh
      margin auto
      background-position center center
      background-repeat no-repeat
      background-size contain
  .dir
    position absolute
    // border 1px solid red
    cursor: pointer
  .NW
    top 0
    left 0
    right 50%
    bottom 50%
  .NE
    top 0
    right 0
    left 50%
    bottom 50%
  .SE
    top 50%
    bottom 0
    right 0
    left 50%
  .SW
    top 50%
    bottom 0
    left 0
    right 50%
</style>