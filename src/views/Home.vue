<template>
  <div class="home">
    <button
      type="button"
      name="button"
      @click="inu()"
      style="margin-bottom: 5px"
    >
      test
    </button>
    <div ref="songle"></div>
    <p>{{ Math.round(this.currentSec) }}</p>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import Songle from "songle-api";
import $ from "jquery";

export default {
  name: "Home",
  data() {
    return {
      currentSec: null,
      sourceUrls: null,
      songIndex: 0,
      Urls: [
        "https://www.youtube.com/watch?v=phwvj0_XM4k",
        "https://www.youtube.com/watch?v=ouLndhBRL4w",
        "https://www.youtube.com/watch?v=JpQBp5iSG6U",
        "https://www.youtube.com/watch?v=gj9RJRkq-gw"
      ],
      startPos: [65, 70, 75, 80]
    };
  },
  components: {
    HelloWorld
  },
  methods: {
    inu() {
      this.sourceUrl = this.Urls[this.songIndex];
      clearInterval(window.timer);
      this.setSongleWidget();
    },
    neko() {
      this.songIndex += 1;
      this.sourceUrl = this.Urls[this.songIndex];
      clearInterval(window.timer);
      this.setSongleWidget();
    },
    setSongleWidget() {
      $(".songle-widget").remove();
      $("#songle-widget-script").remove();

      const songleWidget = document.createElement("div");

      songleWidget.id = "songle-widget";
      songleWidget.setAttribute("class", "songle-widget");
      songleWidget.setAttribute("data-api", "songle-widget-api");
      songleWidget.setAttribute("data-url", this.sourceUrl);

      this.$refs.songle.appendChild(songleWidget);

      // songle widget apiを動的に埋め込んで発火させる
      const script = document.createElement("script");
      script.src = "https://widget.songle.jp/v1/widgets.js";
      script.async = false;
      script.id = "songle-widget-script";
      document.body.appendChild(script);
    }
  },
  mounted() {
    const vm = this;
    this.sourceUrl = this.Urls[this.songIndex];
    this.setSongleWidget();

    window.onSongleWidgetReady = function(apiKey, songleWidget) {
      console.log("songle widget is ready!");
      songleWidget.seekTo(
        SongleWidgetAPI.secondsToMilliseconds(vm.startPos[vm.songIndex])
      ); // Seek to 30 seconds.
      songleWidget.play();

      window.timer = setInterval(function() {
        vm.currentSec = songleWidget.position.seconds;
        if (vm.currentSec > 100) {
          vm.neko();
        }
      }, 1000);
    };

    window.onSongleWidgetError = function(apiKey, songleWidget) {
      console.log("status: " + songleWidget.status);
    };
  }
};
</script>

<style>
.songle-api-license {
  display: none;
}
</style>
