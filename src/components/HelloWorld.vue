<template>
  <div class="container">
    <el-select
      v-model="selectedItem"
      placeholder="请输入用户名"
      filterable
      :loading="loading"
      :remote="true"
      :remote-method="fetchData"
      :value-key="'userName'"
      clearable
      class="custom-select"
      @focus="fetchData(searchQuery)"
    >
      <el-option
        v-for="item in options"
        :key="item.userName"
        :label="item.userName"
        :value="item"
      ></el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      options: [],
      selectedItem: null,
      loading: false,
    };
  },
  methods: {
    fetchData(query) {
      if (query) {
        this.loading = true;
        this.queryUsers(query)
          .then(data => {
            this.options = data;
            this.loading = false;
          })
          .catch(() => {
            this.loading = false;
          });
      } else {
        this.options = [];
      }
    },
    queryUsers(query) {
      return new Promise((resolve) => {
        const allUsers = [
          { userName: 'AA', userAge: 25 },
          { userName: 'Bb', userAge: 30 },
          { userName: 'Cade', userAge: 35 },
          { userName: 'aaa', userAge: 28 },
          { userName: 'ghxihuan', userAge: 22 },
        ];
        const filteredUsers = allUsers.filter(user => 
          user.userName.toLowerCase().includes(query.toLowerCase())
        );
        setTimeout(() => {
          resolve(filteredUsers);
        }, 500);
      });
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.custom-select {
  width: 300px; /* 根据需要调整宽度 */
  border-radius: 5px; /* 圆角 */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* 阴影效果 */
}
</style>