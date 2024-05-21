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
    <div class="create-article__inner">
      <div class="create-article__main">
        <div class="preview-article">
          <form class="preview-article__body card" @submit.prevent="createPost">
            <h5 class="font-weight-bolder">Preview Article</h5>
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
                <h6 class="font-weight-bolder">Add picture</h6>
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
            <div class="body-article">
              <h5 class="font-weight-bolder">Preview Body Article</h5>
              <form class="body-article__form">
                <div class="body-article__title">
                  <h6 class="font-weight-bolder">Title of body article</h6>
                  <input
                    v-model="bodyArticleTitle"
                    type="text"
                    class="form-control form-control-default"
                  />
                </div>

                <div class="body-article__editor">
                  <h6 class="font-weight-bolder">Text area of article</h6>
                  <div>
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
          </form>
        </div>
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
import ImageTool from "@editorjs/image";
import Table from "@editorjs/table";
import Checklist from "@editorjs/checklist";
import Embed from "@editorjs/embed";
import LinkTool from "@editorjs/link";
import CodeTool from "@editorjs/code";
import Quote from "@editorjs/quote";
import Delimiter from "@editorjs/delimiter";
import RawTool from "@editorjs/raw";
import Warning from "@editorjs/warning";
import InlineCode from "@editorjs/inline-code";
import Marker from "@editorjs/marker";
import OpenseaTool from "@editorjs/opensea";
import Underline from "@editorjs/underline";

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
            config: {
              levels: [1, 2, 3, 4, 5, 6],
              defaultLevel: 3,
            },
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
            class: ImageTool,
            config: {
              uploader: {
                uploadByFile(file) {
                  return new Promise((resolve, reject) => {
                    const reader = new FileReader();
                    reader.onload = () => {
                      resolve({
                        success: 1,
                        file: {
                          url: reader.result,
                        },
                      });
                    };
                    reader.onerror = (error) => reject(error);
                    reader.readAsDataURL(file);
                  });
                },
              },
            },
          },
          quote: {
            class: Quote,
            inlineToolbar: true,
            shortcut: "CMD+SHIFT+O",
            config: {
              quotePlaceholder: "Enter a quote",
              captionPlaceholder: "Quote's author",
            },
          },
          Marker: {
            class: Marker,
            shortcut: "CMD+SHIFT+M",
          },
          warning: {
            class: Warning,
            inlineToolbar: true,
            shortcut: "CMD+SHIFT+W",
            config: {
              titlePlaceholder: "Title",
              messagePlaceholder: "Message",
            },
          },
          underline: Underline,
          table: {
            class: Table,
            inlineToolbar: true,
            config: {
              rows: 2,
              cols: 3,
            },
          },
          opensea: {
            class: OpenseaTool,
          },
          checklist: {
            class: Checklist,
            inlineToolbar: true,
          },
          delimiter: Delimiter,
          embed: {
            class: Embed,
            config: {
              services: {
                youtube: true,
                coub: true,
              },
              width: "100%",
            },
          },
          linkTool: {
            class: LinkTool,
            config: {
              endpoint: "http://localhost:8008/fetchUrl", // Your backend endpoint for url data fetching,
            },
          },
          code: CodeTool,
          raw: RawTool,
          inlineCode: {
            class: InlineCode,
            shortcut: "CMD+SHIFT+M",
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
    flex-direction: column;
    gap: 30px;
  }
  &__buttons {
    display: flex;
    justify-content: end;
  }
}
.preview-article {
  width: 100%;
  height: 100vh;
  overflow-y: scroll;
  margin: 40px 0 0 0;
  scrollbar-width: thin;
  scrollbar-color: none;

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
      max-height: 500px;
      object-fit: contain;
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
    padding: 20px 0;
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
  .create-article {
    padding: 0 20px 0 0;
    &__main {
      flex-direction: column;
      padding: 0 0 0 20px;
    }
  }
  .preview-article {
    width: 100%;
  }
  .json-output {
    margin: 30px;
  }
}
</style>
