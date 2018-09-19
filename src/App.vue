<template lang='pug'>
  #app
    .container
      //-main
      .main-area
        label(:class="{active: mirrorMode}") Mirror
          input(type="checkbox", v-model='mirrorMode')
        //- graph，格子區
        .drawing-area(@mousedown="editing = true", @mouseup="editing = false" )
          //- 15行
          .row(v-for='row in settings.height')
            //- 1行有15列
            .block(v-for='column in settings.width'
            ,@click='toggleBlock(column-1,row-1)'
            ,@mousemove='editing? toggleBlock(column-1, row-1, true) : ""'
            ,:class='{active: getGraphData[getArrayId(column-1,row-1)].value == "+"}') 
          //- 影格切換
          .tabs
            .modeTab(v-for='(mode,i) in graphModes', @click='currentModeId = i', :class="{active: currentModeId==i }") {{i+1}}
              .deleteTab(@click.stop= 'removeMode(i)') -
            .modeTab(@click='addMode') +
        
      .controller
        label Width
        input(v-model.number='settings.width',type='number', @change='init')
        label Height
        input(v-model.number='settings.height',type='number' , @change='init')
        textarea(rows=10)
        button Clear

    
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
      // 一個影格有255個小方格
      this.graphModes.push(
        Array.from({length: this.settings.width * this.settings.height},(data,i)=> {
          return {value: '-'}
        })
      )
      this.currentModeId =this.graphModes.length - 1
    },
    getArrayId(column,row) {
      return row * this.settings.width + column
    },
    // 塗黑函數
    toggleBlock (column,row, black) {
      
      let obj = this.getGraphData[ this.getArrayId(column,row) ] 
        
      obj.value = obj.value=='+' ? '-' : '+'
      // console.log(obj.value) 
      if(black) {
        obj.value ='+'
      }
      // 鏡相模式
      if(this.mirrorMode){
        let mid = (this.settings.width - 1) /2
        let newX = mid - (column - mid)
        if(newX != column){
          let obj = this.getGraphData[ this.getArrayId(newX,row) ] 
          obj.value = obj.value=='+' ? '-' : '+'
          // console.log(obj.value) 
          if(black) {
            obj.value ='+'
          }
        }
      }
    },
    // 刪除影格
    removeMode(id){
      if(this.currentModeId >= id){
        this.currentModeId -= 1
      }
      this.graphModes.splice(id,1)
    }
  },
  computed: {
    getGraphData() {
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
      

    .main-area
      flex: 3
      display: flex
      justify-content: center
      flex-direction: column

      label
        +size(70px,30px)
        background-color: #f4f4f4
        color: #333
        font-weight: 900
        border-radius: 5px
        cursor: pointer
        margin-bottom: 1rem
        &.active
          background-color: #333
          color: #f4f4f4
        input
          display: none


    .controller
      flex: 1 0 180px
    .drawing-area

      .row
        display: flex

        .block
          +size(5vmin)
          border: 1px solid #BBB
          background-color: #eee
          cursor: pointer
          &:hover
            background-color: #ccc

          &.active
            background-color: #222

      .tabs
        display: flex
        margin-top: 1rem

        .modeTab
          +size(70px,30px)
          background-color: #f4f4f4
          border-radius: 5px
          margin-right: 1rem
          line-height: 30px
          font-weight: 900
          &.active
            background-color: #333
            color: #f4f4f4

          .deleteTab
            border: 1px solid #f4f4f4
            color: #f4f4f4
            margin-top: 0.5rem
            border-radius: 5px

    .controller
      display: flex
      flex-direction: column
      align-items: center
      color: #eee
      input, textarea
        width: 60%
        margin-bottom: 2rem
</style>
