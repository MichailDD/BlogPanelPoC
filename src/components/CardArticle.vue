<template>
  <div class="card-article card">
    <div class="card-article__info">
      <h3 class="card-article__title">{{ title }}</h3>
      <p class="card-article__descr">
        {{ description }}
      </p>
      <div class="card-article__buttons">
        <button class="btn card-article__button card-article__button-edit">
          Edit
          <i class="fas fa-pencil-alt text-dark me-2" aria-hidden="true"></i>
        </button>
        <button class="btn btn-outline-success">Publish</button>
        <button
          class="btn card-article__button-unpublish text-danger text-gradient btn-outline-danger"
        >
          UnPublish
        </button>
      </div>
    </div>
    <div class="card-article__image">
      <img :src="imageUrl" alt="preview" />
    </div>
  </div>
</template>

<script>
export default {
  props: ["title", "description", "imageUrl"],
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/soft-ui-dashboard/breakpoints";

.card-article {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  max-width: 450px;
  width: 100%;
  padding: 15px;
  gap: 15px;

  &:hover {
    img {
      transform: scale(1.1);
      transition: all 0.3s ease 0s;
    }
  }

  @include media-breakpoint-down(l) {
    max-width: 100%;
  }

  &__title {
    font-size: 20px;

    @include media-breakpoint-down(md) {
      font-size: 18px;
    }
  }
  &__descr {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 4;
    overflow: hidden;
    text-overflow: ellipsis;
    font-size: 13px;
  }

  &__image {
    max-width: 200px;
    max-height: 200px;
    overflow: hidden;
    border-radius: 5px;

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border-radius: 5px;
      transition: all 0.3s ease 0s;
    }
  }

  &__buttons {
    display: flex;
    align-items: center;
    gap: 8px;

    button {
      display: flex;
      align-items: center;
      gap: 7px;
      padding: 10px;

      @include media-breakpoint-down(md) {
        padding: 5px;
        font-size: 10px;
      }
    }
  }
  &__button {
    &-edit {
      border-radius: 10px;
      border: 1px solid grey !important;
    }
  }
}
</style>
