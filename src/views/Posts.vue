<template>
  <div>
    <data-model-crud endpoint="posts">
      <template
        v-slot="{ data: posts, create, read, readAll, update, destroy }"
      >
        <div v-if="!!posts">
          <form>
            <input placeholder="Title" v-model="newPost.title" />
            <input placeholder="Author" v-model="newPost.author" />
            <button
              @click="
                create(newPost);
                readAll();
              "
            >
              Create
            </button>
          </form>

          <div>
            <h1>Posts</h1>
            <div v-for="post in posts" :key="post.id">
              <router-link :to="{ name: 'post', params: { id: post.id } }">
                <span>{{ post.title }}</span>
              </router-link>

              <button
                small
                @click="
                  destroy(post.id);
                  readAll();
                "
              >
                Delete
              </button>
            </div>
          </div>
        </div>
        <div v-else>Loading.........</div>
      </template>
    </data-model-crud>
  </div>
</template>

<script>
import DataModelCrud from "@/components/data-model-crud";
export default {
  components: {
    "data-model-crud": DataModelCrud,
  },
  data() {
    return {
      newPost: {
        title: "",
        author: "",
      },
    };
  },
};
</script>

<style></style>
