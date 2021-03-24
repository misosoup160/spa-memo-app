<template>
  <div id="app">
    <div class="memo">
      <ul class="memo-list">
        <li v-for="memo in memos" :key="memo.id" v-on:click="showMemo(memo.id)">{{ memo.body | title }}</li>
      </ul>
      <span v-on:click="create = true, edit = false" class="memo-add">+</span>
    </div>
    <div class="form">
      <create-input v-if="create" v-model="newMemo" v-on:add="addMemo"></create-input>
      <edit-input v-if="edit" v-model="selectMemo.body" v-on:delete="deleteMemo" v-on:save="editMemo"></edit-input>
    </div>
  </div>
</template>

<script>
import createInput from './components/Create.vue'
import editInput from './components/Edit.vue'

export default {
  name: 'App',
  components: {
    createInput,
    editInput
  },
  data: function () {
    return {
      newMemo: '',
      create: false,
      edit: false,
      selectId: '',
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
    addMemo: function () {
      if (!this.newMemo.match(/\S/g)) {
        return
      }
      const memo = {
        id: Math.random().toString(32).substring(2),
        body: this.newMemo
      }
      this.memos.push(memo)
      this.newMemo = ''
      this.saveMemo()
      this.create = false
    },
    showMemo: function (id) {
      this.selectId = id
      this.edit = true
      this.create = false
    },
    editMemo: function () {
      if (this.selectMemo.body.match(/\S/g)) {
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
