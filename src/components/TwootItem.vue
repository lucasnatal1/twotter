<template>
  <div class="twoot-item" @click="favoriteTwoot(twoot.id)">
    <div class="user-profile__twoot">
      <div class="twoot-item__user">@{{ username }}</div>
      <div class="twoot-item__content">{{ twoot.content }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TwootItem",
  props: {
    username: {
      type: String,
      required: true
    },
    twoot: {
      type: Object,
      required: true
    },
  },
  methods: {
    favoriteTwoot(id) {
      this.$emit('favorite', id);  //create a @ event in the parent (check out UserProfile in the TwooItem tag)
    }
  }
};
</script>

<style>
.twoot-item {
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  box-sizing: border-box;
  cursor: pointer;
  transition: all 0.25s ease;
}

.twoot-item:hover {
  transform: scale(1.07, 1.07);
}

.twoot-item__user {
  font-weight: bold;
  margin: 0 0 3px 0;
}

.twoot-item__content {
  font-family: Segoe UI;
}
</style>