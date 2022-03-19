<template>
  <v-container class="ma-0 pa-0" grid-list-sm>
    <v-subheader> All Blog </v-subheader>

    <v-layout wrap>
      <card-blog
        v-for="blog in blogs"
        :key="`blog-` + blog.id"
        :blog="blog"
      ></card-blog>
    </v-layout>

    <v-pagination
      v-model="page"
      @input="go"
      :length="lengthPage"
      :total-visible="perPage"
    ></v-pagination>
  </v-container>
</template>

<script>
import CardBlog from "../components/CardBlog.vue";

export default {
  data: () => ({
    apiDomain: "https://demo-api-vue.sanbercloud.com",
    blogs: [],
    page: 0,
    lengthPage: 0,
    perPage: 0,
  }),

  components: {
    "card-blog": CardBlog,
  },

  methods: {
    go() {
      const config = {
        method: "get",
        url: this.apiDomain + "/api/v2/blog?page=" + this.page,
      };

      this.axios(config)
        .then((response) => {
          let { blogs } = response.data;
          this.blogs = blogs.data;
          this.page = blogs.current_page;
          this.lengthPage = blogs.last_page;
          this.perPage = blogs.per_page;
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  created() {
    this.go();
  },
};
</script>
