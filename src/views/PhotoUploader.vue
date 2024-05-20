<template>
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
</template>

<script>
export default {
  data() {
    return {
      imageSrc: "",
    };
  },
  props: {
    imageSrc: {
      type:String
    }
  },
  methods: {
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

<style lang="scss" scoped></style>
