<template>
  <div id="app">
    <div class="memo" :class="{inactive: edit}">
      <ul class="memo-list">
        <li v-for="memo in memos" :key="memo.id" @click="showMemo(memo.id)">{{ memo.body | title }}</li>
      </ul>
      <span @click="createMemo" class="memo-add">+</span>
    </div>
    <div class="form">
      <edit-input v-if="edit" v-model="newMemo" @delete="deleteMemo" @edit="editMemo"></edit-input>
    </div>
  </div>
</template>

<script>
import editInput from './components/Edit.vue'

export default {
  name: 'App',
  components: {
    editInput
  },
  data: function () {
    return {
      edit: false,
      selectId: '',
      newMemo: '',
      memos: []
    }
  },
  mounted: function () {
    if (localStorage.getItem('memos')) {
      try {
        this.memos = JSON.parse(localStorage.getItem('memos'))
      } catch(e) {
        localStorage.removeItem('memos')
      }
    }
  },
  methods: {
    saveMemo: function () {
      const parsed = JSON.stringify(this.memos)
      localStorage.setItem('memos', parsed)
    },
    createMemo: function () {
      if (this.edit) {
        return
      }
      const id = Math.random().toString(32).substring(2)
      const newMemo = {
        id: id,
        body: '新規メモ'
      }
      this.memos.push(newMemo)
      this.selectId = id
      this.newMemo = this.selectMemo.body
      this.edit = true
    },
    showMemo: function (id) {
      if (this.edit) {
        return
      }
      this.selectId = id
      this.newMemo = this.selectMemo.body
      this.edit = true
    },
    editMemo: function () {
      if (this.selectMemo.body.match(/\S/g)) {
        this.selectMemo.body = this.newMemo
        this.saveMemo()
        this.edit = false
      } else {
        this.deleteMemo()
      }
    },
    deleteMemo: function () {
      this.memos = this.memos.filter(memo => memo.id !== this.selectId)
      this.saveMemo()
      this.edit = false
    }
  },
  computed: {
    selectMemo: function () {
      return this.memos.find(memo => memo.id === this.selectId)
    }
  },
  filters: {
    title: function (memo) {
      return memo.split('\n')[0]
    }
  }
}
</script>

<style>
* {
 box-sizing: border-box;
}

#app {
  font-family: Verdana, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  width: 500px;
  margin: 0 auto;
  font-size: 14px;
}

.memo {
  width: 200px;
}

.inactive {
  color: #bbb;
  pointer-events: none;
}

.memo-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

.memo-list li {
  cursor: pointer;
  line-height: 1.5;
  margin: 10px 0;
}

.memo-list li:hover {
  color: #42b983;
  transition: all 0.3s;
}

.memo-add {
  cursor: pointer;
  line-height: 1.5;
}
  
.memo-add:hover {
  color: #42b983;
  transition: all 0.3s;
}

.form {
  width: 300px;
}
</style>
