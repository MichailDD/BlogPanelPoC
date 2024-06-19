<template>
  <div class="wrapper">
    <div class="dashboard-articles">
      <CardArticle
        v-for="article in articles"
        :title="article.id"
        :description="article.title"
        :imageUrl="article.url"
      />
    </div>

    <VsudPagination :size="sizePagination" class="pagination">
      <VsudPaginationItem
        :label="'Previous'"
        :prev="true"
        :disabled="isFirstPage"
        @click="previousPage"
      />

      <VsudPaginationItem
        v-for="page in visiblePages"
        :key="page"
        :label="String(page)"
        :active="page === currentPage"
        @click="goToPage(page)"
      />

      <VsudPaginationItem
        :label="'Next'"
        :next="true"
        :disabled="isLastPage"
        @click="nextPage"
      />
    </VsudPagination>
  </div>
  <Loader />
</template>

<script>
import axios from "axios";
import CardArticle from "../components/CardArticle.vue";
import VsudPaginationItem from "../components/VsudPaginationItem.vue";
import Loader from "../components/Loader.vue";
import VsudPagination from "../components/VsudPagination.vue";
export default {
  components: {
    CardArticle,
    VsudPaginationItem,
    Loader,
    VsudPagination,
  },

  data() {
    return {
      articles: [],
      page: 1,
      limit: 9,
      totalPage: 0,
      currentPage: 1,
      visiblePagesRange: 4,
      sizePagination: "lg",
    };
  },
  methods: {
    async getArticles() {
      const response = await axios.get(
        `https://jsonplaceholder.typicode.com/photos`,
        {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        }
      );
      this.totalPage = Math.ceil(
        response.headers["x-total-count"] / this.limit
      );
      this.articles = response.data;
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
      this.page--;
      this.getArticles();
    },
    nextPage() {
      if (this.currentPage < this.totalPage) {
        this.currentPage++;
      }
      this.page++;
      this.getArticles();
    },
    goToPage(page) {
      this.currentPage = page;
      this.page = page;
      this.getArticles();
    },
    paginationSize() {
      const width = window.innerWidth;
      if (width < 1200) {
        this.sizePagination = "md";
      }
    },
  },
  computed: {
    isFirstPage() {
      return this.currentPage === 1;
    },
    isLastPage() {
      return this.currentPage === this.totalPage;
    },
    visiblePages() {
      const halfRange = Math.floor(this.visiblePagesRange / 2);
      let startPage = this.currentPage - halfRange;
      let endPage = this.currentPage + halfRange;

      if (startPage < 1) {
        endPage -= startPage - 1;
        startPage = 1;
      }

      if (endPage > this.totalPage) {
        startPage -= endPage - this.totalPage;
        endPage = this.totalPage;
      }

      const pages = [];
      for (let i = startPage; i <= endPage; i++) {
        pages.push(i);
      }

      return pages;
    },
  },
  mounted() {
    this.getArticles();
    this.paginationSize();
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/soft-ui-dashboard/breakpoints";

.dashboard-articles {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  justify-content: center;
  gap: 15px;
  padding: 0 15px 15px 0;

  @include media-breakpoint-down(l) {
    padding-left: 15px;
  }
}
.pagination {
  display: flex;
  justify-content: center;
  gap: 5px;
}
.wrapper {
  padding-bottom: 30px;
}
</style>
