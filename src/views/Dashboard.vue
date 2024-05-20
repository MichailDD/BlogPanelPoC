<template>
  <div>
    <post-list :previewArticle="creativeArticle" />
  </div>
</template>

<script>
import PostPreview from "./PostPreview.vue";
import PostList from "./PostList.vue";
import CreateArticle from "./CreateArticle.vue";
export default {
  name: "DashboardDefault",
  data() {
    return {
      creativeArticle: [],
    };
  },
  components: {
    PostPreview,
    PostList,
    CreateArticle,
  },
};
</script>
