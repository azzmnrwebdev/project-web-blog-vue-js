<template>
  <v-container class="ma-0 pa-0" grid-list-sm>
    <div class="text-right">
      <v-btn small text to="/blogs" class="blue--text">
        All Blog <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </div>

    <v-layout wrap>
      <card-blog
        v-for="blog in blogs"
        :key="`blog-` + blog.id"
        :blog="blog"
      ></card-blog>
    </v-layout>
  </v-container>
</template>

<script>
import CardBlog from "../components/CardBlog.vue";

export default {
  data: () => ({
    apiDomain: "https://demo-api-vue.sanbercloud.com",
    blogs: [],
  }),

  components: {
    "card-blog": CardBlog,
  },

  methods: {
    go() {
      const config = {
        method: "get",
        url: this.apiDomain + "/api/v2/blog/random/4",
      };

      this.axios(config)
        .then((response) => {
          let { blogs } = response.data;
          this.blogs = blogs;
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
