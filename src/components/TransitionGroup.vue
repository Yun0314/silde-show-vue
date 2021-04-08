<template>
  <div>
    <div class="image-list-wrap" :style="viewWrapWidth">
      <transition-group class="image-list" name="image" tag="div" ref="view">
        <div class="image" v-for="(image, i) in images" :key="image">
          <img :src="image" alt="" />
          <a
            v-if="clear"
            class="image-remover"
            href="#"
            @click.prevent="$emit('remove', i)"
          >
            &times;
          </a>
        </div>
      </transition-group>

      <div v-if="!!!images.length" class="no-image">尚未加入圖片</div>
    </div>

    <div class="image-buttons-block">
      <button
        class="image-prev"
        :class="{ disable: prevButtonDisable }"
        :disabled="prevButtonDisable"
        @click.prevent="currentIndexMinus()"
      >
        <span class="material-icons" v-text="'arrow_back'" />
      </button>

      <button
        class="image-next"
        :class="{ disable: nextButtonDisable }"
        :disabled="nextButtonDisable"
        @click.prevent="currentIndexPlus()"
      >
        <span class="material-icons" v-text="'arrow_forward'" />
      </button>
    </div>
    <p class="image-desc">{{ currentIndex }} / {{ images.length }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentIndex: 0
    };
  },
  computed: {
    // 8是css中image的padding
    movedDistance() {
      return `translateX(${this.currentIndex * -1 * (this.imageWidth + 8)}px)`;
    },
    viewWrapWidth() {
      return { width: (this.imageWidth + 8) * this.perPage + "px" };
    },
    pageIdReference() {
      return this.images.length - this.perPage;
    },
    prevButtonDisable() {
      return !this.currentIndex;
    },
    nextButtonDisable() {
      return (
        this.images.length <= this.perPage ||
        this.currentIndex >= this.pageIdReference
      );
    }
  },
  watch: {
    movedDistance(val) {
      this.$refs["view"].$el.style.transform = val;
    },
    //新增時到新圖片的定點
    pageIdReference(val, oldVal) {
      if (oldVal < val && !this.nextButtonDisable) {
        this.currentIndex = val;
      }
    },
    "images.length"(val, oldVal) {
      val > oldVal ? this.currentIndexPlus() : this.currentIndexMinus();
    }
  },
  methods: {
    currentIndexPlus() {
      this.nextButtonDisable || this.currentIndex++;
    },
    currentIndexMinus() {
      this.prevButtonDisable || this.currentIndex--;
    }
  },
  props: {
    images: {
      type: Array,
      default: () => {
        return [];
      }
    },
    imageWidth: {
      type: Number,
      default: 200
    },
    perPage: {
      type: Number,
      default: 3
    },
    // 是否顯示刪除照片按鈕
    clear: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style lang="less" scoped>
.image-list {
  display: flex;
  transition: transform 100ms ease-in-out;
}

.no-image {
  width: inherit;
  display: flex;
  color: rgb(201, 201, 201);
  align-items: center;
  justify-content: center;
  font-size: 40px;
  font-weight: bold;
}

.image-list-wrap {
  display: flex;
  height: 310px;
  overflow: hidden;
}

.image {
  position: relative;
  padding: 0 4px;
}

.image-remover {
  position: absolute;
  top: 8px;
  right: 8px;
  text-decoration: none;
  background-color: rgba(27, 27, 27, 0.24);
  width: 30px;
  height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: @white;
  border-radius: 99rem;
  font-size: 20px;
}

.image-buttons-block {
  display: flex;
  justify-content: center;
  padding: 8px 0;
}

.image-prev,
.image-next {
  display: flex;
  margin: 0 5px;
  width: 100px;
  justify-content: center;
  align-items: center;
}

// transition
.image-enter {
  transform: translateY(100%);
  opacity: 0;
}

.image-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}

.image-leave-active ~ .image {
  transition: transform 250ms ease;
  transform: translateX(-100%);
}

.image-leave-active,
.image-enter-active {
  transition: all 250ms ease;
}

.image-desc {
  padding: 0 35px;
  color: @gray;
}
</style>
