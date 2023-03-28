<template>
  <div id="group" class="display-flex">
    <div
      id="div1"
      class="header"
      @mousedown="mousedown"
      @mousemove="mousemove"
      @mouseup="mouseup"
      data-tauri-drag-region
    ></div>
    <!-- <div id="div2" class="div-size">
      <a-select style="width: 90px" placeholder="请选择">
        <a-select-option selected value="1">药名</a-select-option>
        <a-select-option value="2">指南</a-select-option>
        <a-select-option value="1">药名</a-select-option>
        <a-select-option value="2">指南</a-select-option>
        <a-select-option value="1">药名</a-select-option>
        <a-select-option value="2">指南</a-select-option>
      </a-select>
    </div> -->
    <div id="div3" class="div-right-size">
      <a-input-search
        v-model:value="drugName"
        placeholder="请输入药品名称"
        enter-button
        style="width: 200px"
        @search="handleSearch"
      />
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, reactive, ref, unref } from "vue";

import { WebviewWindow } from "@tauri-apps/api/window"; //引入打开窗口功能

export default defineComponent({
  name: "FloatTools",
  components: {},
  setup() {
    let isMove = false;
    let baseX = 100,
      baseY = 200; //监听坐标
    // const ipcRenderer = reactive({ ...window.electron.ipcRenderer })

    const drugName = ref();
    async function handleSearch() {
      console.log("newWindow" + drugName.value);
      if (!unref(drugName)) {
        //ipcRenderer.send('openWindow')
        return;
      }
      // ipcRenderer.send('openWindow', drugName.value)
      const webview2 = new WebviewWindow("theUniqueLabel", {
        url: "https://www.yixue.com/" + drugName.value,
        width: 1400,
        height: 800,
        title: "医睦桌面助手",
        // 可自行添加属性配置     窗口配置
        //center:true,
        //decorations:false
      });
      webview2.once("tauri://created", function () {
        // webview window successfully created
        // 窗口创建成功 打印1
        console.log(1);
      });
      webview2.once("tauri://error", function (e) {
        // an error happened creating the webview window
        // 窗口创建失败 打印2
        console.log(e);
      });
    }

    //鼠标按下事件
    function mousedown(event: any): void {
      //鼠标按下事件
      baseX = event.x;
      baseY = event.y;
      isMove = true;
    }

    //鼠标移动
    function mousemove(event: any): void {
      if (!isMove) {
        return;
      }
      const x = event.screenX - baseX;
      const y = event.screenY - baseY;

      // ipcRenderer.send('move-application', {
      //   posX: x,
      //   posY: y
      // })
    }

    // //鼠标抬起
    function mouseup(): void {
      isMove = false;
    }

    return {
      handleSearch,
      mousedown,
      mousemove,
      mouseup,
      drugName,
    };
  },
});
</script>

<style lang="less">
//@import '../assets/css/styles.less';

// #group {
//   display: flex;
//   justify-content: flex-start;
// }

.display-flex {
  display: flex;
}

.div-size {
  margin-left: 60px;
  z-index: 1;
}
.div-right-size {
  margin-left: 100px;
  margin-top: 7px;
}
.header {
  background: url(../assets/hederlogo1.png) no-repeat 4px 5px;
  width: 70px;
  height: 60px;
  position: fixed;
  top: 5px;
  left: 5px;
}
</style>
