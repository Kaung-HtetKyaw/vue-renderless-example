<template>
  <div>
    <data-model-crud endpoint="posts" :postID="$route.params.id">
      <template v-slot="{ data: post, create, read, readAll, update, destroy }">
        <div v-if="!!post">
          <form>
            <input placeholder="Title" v-model="post.title" />
            <input placeholder="Author" v-model="post.author" />
            <button @click="update(post.id, post)">Update</button>
            <button
              @click="
                destroy(post.id);
                readAll();
              "
            >
              Delete
            </button>
          </form>
        </div>
        <div v-else>Loading..........</div>
      </template>
    </data-model-crud>
  </div>
</template>

<script>
import DataModelCrud from "@/components/data-model-crud";
export default {
  components: {
    DataModelCrud,
  },
};
</script>

<style></style>
