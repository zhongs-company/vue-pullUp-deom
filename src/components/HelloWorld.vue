<template>
	<div class="hello">
		<div id="listloading">
			<div>
				<ul id="order-list" class="order-list">

				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import Listloading from "listloading";
var m = 3;
var n = 0;
// 模板
var createHtml = function() {
  var __html = "";
  for (var i = 0; i < 15; i++) {
    var now = new Date().getTime();
    now = new Date(now + i * 1000000);

    __html +=
      '<li><span class="icon"></span><p class="title"><time class="r">' +
      now.getHours() +
      ":" +
      now.getMinutes() +
      ":" +
      now.getSeconds() +
      "</time>listloading" +
      n++ +
      '</p><p class="text">移动端上拉下拉刷新组件...</li>';
  }
  return __html;
};

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  },
  mounted() {
    // 创建iscroll之前必须要先设置父元素的高度，否则无法拖动iscroll
    var hei = $(window).height();
    $("#listloading, .listloadingClass").height(hei);
    // demo
    var listloading = new Listloading("#listloading", {
      disableTime: true, // 是否需要显示时间
      pullUpAction: function(cb) {
        // 上拉加载更多
        m--;
        var flg = false;
        var __html = createHtml();
        if (m < 1) {
          flg = true;
        } else {
          $("#order-list").append(__html);
        }
        // 数据加载完毕需要返回 end为true则为全部数据加载完毕
        cb(flg);
      },
      pullDownAction: function(cb, flg) {
        // 下拉刷新
        // true则为默认加载 false为下拉刷新
        if (flg) {
          console.log("默认加载");
        }
        m = 3;
        var __html = createHtml();
        $("#order-list").html(__html);
        // 执行完执行方法之后必须执行回调 回调的作用是通知默认加载已经全部执行完毕，程序需要去创建iscroll或者做下拉刷新动作
        cb();
      },
      // iscroll的API
      iscrollOptions: {
        //
      }
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import '../assets/listloading.min.scss'
</style>
