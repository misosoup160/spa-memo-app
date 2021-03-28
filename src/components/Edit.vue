<template>
  <div class="form">
    <textarea :value="value" @input="$emit('input', $event.target.value)" v-focus></textarea>
    <div class="btn">
      <button class="btn-edit" @click="$emit('edit')">編集</button>
      <button class="btn-delete" @click="$emit('delete')">削除</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'editInput',
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
  display: flex;
  justify-content: space-around;
  margin: 15px 0;
  height: 35px;
}

.btn-edit {
  width: 200px;
}

.btn-delete {
  width: 50px;
}
</style>
