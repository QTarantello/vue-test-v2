<template>
  <div class="table">
    <div class="table-head">
      <div @click="$emit('onSortUsers', 'name')" class="table-head__cell">
        Имя
        <span v-if="sortBy.key === 'name'">
          {{ sortBy.direction === "asc" ? "▼" : "▲" }}
        </span>
      </div>
      <div @click="$emit('onSortUsers', 'phone')" class="table-head__cell">
        Телефон
        <span v-if="sortBy.key === 'phone'">
          {{ sortBy.direction === "asc" ? "▼" : "▲" }}
        </span>
      </div>
    </div>

    <div class="table-body">
      <div v-for="user in users" :key="user.uuid">
        <div class="table-row">
          <div @click="user.children.length !== 0 ? showChild() : null" class="table-row__cell">
            <span v-if="user.children.length !== 0">+</span>
            {{ user.name }}
          </div>
          <div class="table-row__cell">{{ user.phone }}</div>
        </div>
        <div v-for="child in user.children" :key="child.uuid">
          <div v-if="visible === true" class="table-row--child">
            <div class="table-row__cell--child">+ {{ child.name }}</div>
            <div class="table-row__cell--child">{{ child.phone }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sortBy: {
      type: Object
    },
    users: {
      type: Array
    },
    onSortUsers: {
      type: Function
    }
  },
  data () {
    return {
      visible: false
    }
  },
  methods: {
    showChild () {
      this.visible = !this.visible
    }
  }
}
</script>

<style scoped>
.table {
  width: 600px;
  margin: 0 auto;
}

.table-head {
  display: flex;
  flex-direction: row;
  border: 1px solid rgb(203, 203, 203);
  background-color: rgb(224, 224, 224);
}

.table-row__cell--child,
.table-head__cell {
  width: 300px;
  padding: 5px;
  border-left: 1px solid rgb(203, 203, 203);
  cursor: pointer;
}

.table-row__cell--child:first-of-type,
.table-head__cell:first-of-type {
  border: 0;
}

.table-row--child,
.table-row {
  display: flex;
  flex-direction: row;
  margin: -1px 0 0 0;
  border: 1px solid rgb(203, 203, 203);
}

.table-row__cell {
  width: 300px;
  padding: 5px;
  border-left: 1px solid rgb(203, 203, 203);
}

.table-row__cell:first-of-type {
  border: 0;
}

.table-row--child {
  margin: 0 0 0 50px;
}

.table-row--child:first-of-type {
  border-top: 0;
}

.table-row__cell--child:first-of-type {
  width: 249px;
}
</style>
