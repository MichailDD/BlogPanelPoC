<template>
  <div class="create-article py-4 container-fluid">
    <div class="create-article__buttons">
      <button
        @click="createPost"
        class="btn mb-0 bg-gradient-dark btn-md null null"
      >
        Save
      </button>
    </div>
    <div class="create-article__main">
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
            <div class="card h-100">
              <input
                placeholder="Upload"
                class="form-control form-control-default preview-article__photo-input"
                name="file"
                type="file"
                @change="handleFileUpload"
                ref="fileInput"
              />
              <img
                class="preview-article__photo-image"
                :src="imageSrc"
                v-if="imageSrc"
                alt="Uploaded Image"
              />
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
      <div class="body-article">
        <h5 class="font-weight-bolder">Preview Body Article</h5>
        <form class="body-article__form card">
          <div class="body-article__title">
            <h6 class="font-weight-bolder">Title of body article</h6>
            <input
              v-model="bodyArticleTitle"
              type="text"
              class="form-control form-control-default"
            />
          </div>
          <div class="body-article__editor">
            <div>
              <div></div>

              <div class="editorx_body">
                <div
                  id="codex-editor"
                  class="bg-gray-100 body-article__editor-canvas"
                />
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="json-output" v-if="latestPost">
    <h5 class="font-weight-bolder">Latest Post JSON</h5>
    <pre>{{ latestPost }}</pre>
  </div>
</template>

<script>
import PreviewArticle from "./PreviewArticle.vue";
import BodyArticle from "./BodyArticle.vue";
import PostList from "./PostList.vue";
import EditorJS from "@editorjs/editorjs";
import Header from "@editorjs/header";
import Paragraph from "@editorjs/paragraph";
import List from "@editorjs/list";
import InlineImage from "editorjs-inline-image";

export default {
  components: {
    PreviewArticle,
    BodyArticle,
    PostList,
  },
  data() {
    return {
      title: "",
      picAlt: "",
      shortDescriptions: "",
      imageSrc: "",
      creativeArticle: [],
      bodyArticleTitle: "",
      editor: null,
      latestPost: null,
    };
  },
  methods: {
    createPost() {
      this.editor.save().then((savedData) => {
        let newPost = {
          previeArticle: {
            title: this.title,
            pictureUrl: this.imageSrc,
            picAlt: this.picAlt,
            shortDescriptions: this.shortDescriptions,
          },
          bodyArticle: {
            title: this.bodyArticleTitle,
            reductorObject: savedData,
          },
        };
        this.creativeArticle.push(newPost);
        this.latestPost = JSON.stringify(newPost, null, 2);
        console.log(this.latestPost);
        this.title = "";
        this.picAlt = "";
        this.imageSrc = "";
        this.shortDescriptions = "";
        this.bodyArticleTitle = "";
        this.editor.clear();
      });
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
    myEditor() {
      this.editor = new EditorJS({
        holder: "codex-editor",
        autofocus: true,
        initialBlock: "paragraph",
        tools: {
          header: {
            class: Header,
            shortcut: "CMD+SHIFT+H",
          },
          list: {
            class: List,
          },
          paragraph: {
            class: Paragraph,
            config: {
              placeholder: ".",
            },
          },
          image: {
            class: InlineImage,
            inlineToolbar: true,
            config: {
              embed: {
                display: true,
              },
            },
          },
        },
      });
    },
  },
  mounted() {
    this.myEditor();
  },
};
</script>

<style lang="scss" scoped>
.create-article {
  padding: 0 40px 0 0;
  &__main {
    display: flex;
    justify-content: space-between;
    gap: 30px;
  }
  &__buttons {
    display: flex;
    justify-content: end;
  }
}
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

    &-input {
      padding-left: 5px !important;
    }

    &-image {
      max-width: 100%;
      max-height: 300px;
      object-fit: cover;
      width: 100%;
      height: 100%;
      margin: 30px 0 15px 0;
      border-radius: 20px;
    }
  }
}
.body-article {
  width: 100%;

  &__form {
    padding: 20px 15px;
    display: flex;
    flex-direction: column;
    gap: 25px;

    input {
      padding-left: 5px !important;
    }
  }

  &__editor {
    &-canvas {
      border-radius: 3px;
      padding: 15px 20px;
    }
  }
}
.json-output {
  padding: 20px;
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 5px;
  margin: 30px 30px 30px 0;
  width: auto;

  pre {
    font-size: 14px;
    width: auto;
  }
}
@media (max-width: 1201px) {
  .create-article__main {
    flex-direction: column;
    padding: 0 0 0 30px;
  }
  .preview-article {
    width: 100%;
  }
  .json-output {
    margin: 30px;
  }
}
</style>
