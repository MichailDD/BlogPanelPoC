<template>
  <button
    @click="createPost"
    class="btn mb-0 bg-gradient-dark btn-md null null w-10"
  >
    Save
  </button>
  <div class="preview-article">
    <h5 class="font-weight-bolder">Preview Article</h5>
    <form class="preview-article__body card" @submit.prevent="createPost">
      <div class="preview-article__title">
        <h6 class="font-weight-bolder">Title of article</h6>
        <input
          v-model="title"
          type="text"
          class="form-control form-control-default preview-article__title-input"
        />
      </div>
      <div class="preview-article__photo">
        <div class="border card h-100">
          <input
            placeholder="Upload"
            class="input"
            type="file"
            @change="handleFileUpload"
            ref="fileInput"
          />
          <img :src="imageSrc" v-if="imageSrc" alt="Uploaded Image" />
          <button
            class="btn btn-link text-danger text-gradient px-3 mb-0"
            @click="clearImage"
            v-if="imageSrc"
          >
            Delete photo
          </button>
        </div>
        <div class="preview-article__photo-descr">
          <h6 class="font-weight-bolder">Picture of description</h6>
          <input
            v-model="picAlt"
            type="text"
            class="form-control form-control-default preview-article__title-input"
          />
        </div>
        <div class="preview-article__photo-text">
          <h6 class="font-weight-bolder">Short description</h6>
          <textarea
            v-model="shortDescriptions"
            class="form-control preview-article__title-input"
          ></textarea>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import PhotoUploader from "./PhotoUploader.vue";
import PostList from "./PostList.vue";
export default {
  components: {
    PhotoUploader,
    PostList,
  },
  data() {
    return {
      title: "",
      picAlt: "",
      shortDescriptions: "",
      previeArticle: [],
      imageSrc: "",
    };
  },
  methods: {
    createPost() {
      let newPost = {
        id: Date.now(),
        title: this.title,
        pictureUrl: this.imageSrc,
        picAlt: this.picAlt,
        shortDescriptions: this.shortDescriptions,
      };
      this.previeArticle.push(newPost);
      console.log(newPost);
      this.title = "";
      this.picAlt = "";
      this.imageSrc = "";
      this.shortDescriptions = "";
    },

    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.imageSrc = URL.createObjectURL(file);
      }
    },
    clearImage() {
      this.imageSrc = null;
      const input = this.$refs.fileInput;
      if (input) {
        input.value = null;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.preview-article {
  width: 65%;

  &__body {
    padding: 20px 15px;
    display: flex;
    flex-direction: column;
    gap: 25px;
  }

  &__title {
    &-input {
      padding-left: 5px !important;
    }
  }

  &__photo {
    display: flex;
    flex-direction: column;
    gap: 25px;
  }
}
</style>
