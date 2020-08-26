<template>
  <div class="page">
    <el-container>
      <el-header>Header</el-header>
      <el-container>
        <el-aside width="200px">Aside</el-aside>
        <el-main style="padding: 0;overflow: hidden;">
          <div style="display: flex;flex-direction: column;height: 100%;">
            <div style="background:whitesmoke;height: 60px;line-height: 60px;flex: 0;"></div>
            <div style="background:whitesmoke;flex: 1;"></div>
            <div style="flex: none;position: relative;padding: 45px 0 5px 0;border: 1px solid gainsboro;background: white;">
              <div class="icon-button">
                <el-popover placement="top-start" width="400" trigger="click" v-model="emojiPanel">
                  <el-tabs type="border-card">
                    <el-tab-pane v-for="(emojiGroup, category) in Emojis" :key="category" :label="category">
                      <span class="no-select" v-for="(emoji, emojiName) in emojiGroup" :key="emojiName" @click="insert(emoji)" :title="emojiName">{{ emoji }}</span>
                    </el-tab-pane>
                  </el-tabs>
                  <el-button slot="reference"  size="mini" circle><i style="font-size: 13px;" class="iconfont icon-smile"></i></el-button>
                </el-popover>
                <el-tooltip content="图片" placement="bottom-start" style="margin-left: 10px;">
                  <el-button  size="mini" icon="el-icon-picture-outline-round" circle ></el-button>
                </el-tooltip>
              </div>
              <div class="m-text">
                <textarea class="tarea" ref="tarea" @blur="getBlur"  placeholder="按 Enter 发送" v-model="textarea"></textarea>
              </div>
              <el-tooltip content="发送" placement="bottom-start"><el-button  size="small" class="sendButton" >发送</el-button></el-tooltip>
            </div>
          </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import Emojis from '@/assets/js/emojis'
export default {
  name: 'Index',
  data () {
    return {
      textarea: '',
      cursorPos: 0,
      emojiPanel: false,
      Emojis: Emojis, // 本地表情文件
    }
  },
  methods: {
    insert (emoji) { // 添加表情
      let textDom = this.$refs.tarea
      var startPos = textDom.selectionStart
      var endPos = textDom.selectionEnd
      var scrollTop = textDom.scrollTop
      this.textarea = this.textarea.substring(0, startPos) + emoji + this.textarea.substring(endPos, this.textarea.length)
      setTimeout(() => {
        textDom.focus()
        textDom.selectionStart = this.cursorPos + 2 // 光标起始选择区域
        textDom.selectionEnd = this.cursorPos + 2 // 光标结尾选择区域
        textDom.scrollTop = scrollTop // 光标行高度
      }, 50)
      this.emojiPanel = false
    },
    getBlur () { // 文本框失焦
      var element = this.$refs.tarea
      let cursorPos = 0
      if (document.selection) {
        var selectRange = document.selection.createRange()
        selectRange.moveStart('character', -element.value.length)
        cursorPos = selectRange.text.length
      } else if (element.selectionStart || element.selectionStart === '0') {
        cursorPos = element.selectionStart
      }
      this.cursorPos = cursorPos
    }
  }
}
</script>

<style>
.page {
    position: absolute;
    top: 0px;
    right: 0px;
    bottom: 0px;
    left: 0px;
    margin: 50px 250px 50px 250px;
}
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
.el-aside {
  background-color: #d3dce6;
  color: #333;
  text-align: center;
  line-height: 200px;
}
.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
}
.el-container {
  height: 100%;
}
.icon-button {
  position: absolute;
  top: 7px;
  left: 15px;
}
.m-text {
  height: 120px;
  width: 100%;
  padding: 0 15px;
  box-sizing: border-box;
}
.sendButton {
  position: absolute;
  top: 7px;
  right: 30px;
}
.tarea::-webkit-scrollbar {
  display: none;
}
textarea {
  height: 100%;
  width: 100%;
  border: none;
  outline: none;
  font-family: 'Micrsofot Yahei';
  resize: none;
}
</style>