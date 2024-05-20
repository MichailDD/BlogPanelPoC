<template>
  <div
    class="py-4 container-fluid"
    v-for="post in previewArticle"
    :key="post.id"
  >
    <div class="row">
      <div class="col-lg-6 mb-lg-0 mb-4">
        <div class="card">
          <div class="card-body p-3">
            <div class="row">
              <div class="col-lg-6">
                <div class="d-flex flex-column h-100">
                  <!-- <p class="mb-1 pt-2 text-bold">Built by developers</p> -->
                  <h5 class="font-weight-bolder">{{ post.title }}</h5>
                  <p class="mb-5">
                    {{ post.shortDescriptions }}
                  </p>
                </div>
              </div>
              <div class="col-lg-5 ms-auto text-center mt-5 mt-lg-0">
                <!-- Image -->
                <img
                  :src="post.pictureUrl"
                  v-if="post.pictureUrl"
                  alt="Post Image"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    previewArticle: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped></style>
