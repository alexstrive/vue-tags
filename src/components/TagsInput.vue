<template>
  <div class="tags-input-wrapper">
    <span class="tag-item" v-for="(tag, index) in tags" :key="index">
      {{ tag }}
      <a href="#" @click.prevent="removeTag(index)">&times;</a>
    </span>
    <input
        type="text"
        class="tag-input"
        v-model.trim="newTag"
        @keydown.enter="addNewTag"
        @keydown.delete="removeLastTag"
        @keydown.tab.prevent="addNewTag"
        :class="{'tag-exists': isTagExists}"
    />
  </div>
</template>

<script>
export default {
  name: "TagsInput",
  data: () => ({
    tags: ["vue", "react", "angular"],
    newTag: ""
  }),
  watch: {
    newTag(newVal) {
      if (newVal.indexOf(",") > -1) {
        this.newTag = this.newTag.slice(0, -1);
        this.addNewTag();
      }
    }
  },
  computed: {
    isTagExists() {
      return this.tags.includes(this.newTag);
    }
  },
  emits: ["change"],
  methods: {
    removeTag(index) {
      this.tags.splice(index, 1);
      this.$emit("change", this.tags);
    },
    addNewTag() {
      if (this.newTag && !this.isTagExists) {
        this.tags.push(this.newTag);
        this.newTag = "";
        this.$emit("change", this.tags);
      }
    },
    removeLastTag() {
      if (this.newTag.length === 0) {
        this.removeTag(this.tags.length - 1);
      }
    }
  }
}
</script>

<style scoped>
.tag-input.tag-exists {
  color: red;
  text-decoration: line-through;
}

.tags-input-wrapper {
  background: #fff;
  padding: 0.5em;
  border: 1px solid #dbdbdb;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  min-height: 36px;
  box-sizing: border-box;
}

.tag-item {
  color: #212529;
  background-color: #eee;
  margin-right: 0.3em;
  font-size: 75%;
  line-height: 1;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0.25em 1.25em 0.25em 0.6em;
}

.tag-input {
  color: #495057;
  flex: 1;
  background: transparent;
  border: none;
}

.tag-input:focus {
  outline: none;
}

a.remove-tag {
  text-decoration: none;
}
</style>