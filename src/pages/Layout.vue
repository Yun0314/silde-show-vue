<template>
  <div>
    <h1>SlideShow</h1>

    <TransitionGroup
      :images="images"
      :imageWidth="imageWidth"
      :perPage="perPage"
      :clear="true"
      @remove="removePic"
    />

    <button class="image-adder" @click="addPic">
      <span class="material-icons" v-text="'add_photo_alternate'" />
    </button>
  </div>
</template>

<script>
import TransitionGroup from "@/components/TransitionGroup";
export default {
  data() {
    return {
      images: [],
      imageWidth: 400,
      perPage: 3
    };
  },
  components: { TransitionGroup },
  methods: {
    addPic() {
      // 借用類星體的萬用圖片:D 推廣: https://quasar.dev/start/quasar-cli
      let url =
        `https://placeimg.com/${this.imageWidth}/300/nature?t=` + Math.random();

      var imageDom = new Image();
      imageDom.onload = e => {
        this.images.push(url);
      };
      imageDom.src = url;
    },
    removePic(i) {
      this.images.splice(i, 1);
    }
  }
};
</script>

<style lang="less">
.image-adder {
  padding: 0px 40px;
  height: 30px;
}
</style>
