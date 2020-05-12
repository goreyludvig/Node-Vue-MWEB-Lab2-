// Форма для відображення книги
<template>
  <form v-if="visible" @submit.prevent>
    <!-- якщо форма видима то показати її і відмінити надсилання запиту за замовчуванням-->
    <label>
      Ім'я
      <input type="text" v-model="dai.name" required />
    </label>
    <br />
    <label>
      Марка
      <input type="text" v-model="dai.mark" required />
    </label>
    <br />
    <label>
      Номер
      <input type="number" v-model="dai.number" />
    </label>
    <br />
    <label>
      Рік випуску
      <input type="number" v-model.number="dai.year" min="1885" step="1" />
    </label>
    <br />
    <input type="button" @click="save()" value="Зберегти" />
  </form>
</template>

<script>
import setInput from "./setInput";

export default {
  name: "daiForm",
  props: {
    value: Object,
    visible: Boolean
  },
  data() {
    return {
      dai: this.value
    };
  },
  components: {
    setInput
  },
  methods: {
    save() {
      this.$emit("input", this.dai);
    }
  }
};
</script>
<style scoped>
form {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: white;
}
</style>