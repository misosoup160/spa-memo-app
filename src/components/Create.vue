<template>
  <form v-on:submit.prevent="$emit('add')" class="form">
    <textarea v-bind:value="value" v-on:input="$emit('input', $event.target.value)" v-focus></textarea>
    <input type="submit" value="追加" class="btn">
  </form>
</template>

<script>
export default {
  name: 'createInput',
  props: {
    value: String
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style scoped>
.form {
  width: 300px;
}

.form textarea {
  display: block;
  margin: 0;
  width: 100%;
  height: 400px;
  padding: 12px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 8px;
  color: #2c3e50;
}

.btn {
  display: block;
  margin: 15px auto;
  height: 35px;
  width: 200px;
}
</style>
