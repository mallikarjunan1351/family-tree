<template>
  <div
    class="custom-card"
    v-bind:class="gender === 'male' ? 'male' : 'female'"
    @click.prevent="click"
  >
    <div
      :style="{
        backgroundImage: image ? `url(${image})` : null,
      }"
      class="custom-card__image"
    />
    <div class="custom-card__info">
      <div class="custom-card__name">
        {{ name }}
      </div>
      <div v-show="age" class="custom-card__age">Age: {{ age }}</div>
      <div v-show="relationship" class="custom-card__age">
        {{ relationship }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FamilyCard",
  props: {
    name: String,
    image: String,
    age: String,
    gender: String,
    relationship: String,
    source: Object,
  },
  methods: {
    click() {
      this.$emit("click", this.source);
    },
  },
};
</script>

<style lang="scss" scoped>
.vue-family-card {
  text-align: center;
  width: 100px;
  &__image {
    display: block;
    width: 100px;
    height: 100px;
    margin-bottom: 16px;
    background-position: 50%;
    background-size: cover;
    background-color: #dedede;
  }
  &__name {
    font-weight: 600;
    a {
      color: #000;
      text-decoration: none;
    }
  }
}
</style>