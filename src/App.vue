<template lang='pug'>
  #app
    .container
      //-main
      .main-area
        //- graph
        .drawing-area
          //- 15行
          .row(v-for='row in settings.height')
            //- 15列
            .block(v-for='column in settings.width')
        
      .controller
      
    
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      graphModes: [],
      settings: {
        width: 15,
        height: 15
      },
      mirrorMode: true,
      currentModeId: 0,
      editing: false
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      // 回到第一個影格
      this.currentModeId = 0
      this.graphModes = []
      // 加入新影格
      this.addMode()
    },
    addMode() {
      // 產升影格資料
      this.graphModes.push(
        Array.from({length: this.settings.width * this.settings.height},(data,i)=> {
          return {value: '-'}
        })
      )
      this.currentModeId =this.graphModes.length - 1
    },
    getArrayId(column,row) {
      return row * this.width + column
    },
    toggleBlock (column,row) {

    }
  },
  computed: {
    graphData() {
      return this.graphModes[this.currentModeId]
    }
  }
}
</script>

<style lang="sass">
@mixin size($w,$h:$w)
  width: $w
  height: $h

html, body
  background-color: #111
  color: #eee

#app 
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px
  width: 100%

  .container
    display: flex

    .main-area, .controller
      padding: 10px
      flex: 1

    .drawing-area

      .row
        display: flex

        .block
          +size(5vmin)
          border: 1px solid #666
          background-color: #eee
          cursor: pointer
          &:hover
            background-color: #ccc

          &.active
            background-color: #333


</style>
