<template>
  <div class="p-4 space-y-2">
    
    <div class="flex items-center space-x-2">
      <h1 @click="getUsers(page)" v-for="page in totalPages" :key="page" :class="{'cursor-pointer underline text-red-500' : page != currentPage}">{{page}}</h1>
    </div>
    
    <div>
      <input v-model="searchTerm" class="border p-2" placeholder="Search..." />
    </div>

    

    <div>
      <div v-for="user in filteredUsers" :key="user.id" class="flex items-center space-x-4">
        <div>
          <img :src="user.avatar" />
        </div>
        <div>
          <h1>{{user.email}}</h1>
          <h1>{{user.first_name}}</h1>
          <h1>{{user.last_name}}</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  components: {
    
  },
  mounted(){
    this.getUsers(1);
  },
  methods:{
    async getUsers(page){
      if(this.currentPage == page)
        return false;
      let data  = await axios.get('https://reqres.in/api/users',{
        params:{
          per_page: 4,
          page
        }
      }).then(res => res.data)

      this.users = data.data;
      this.currentPage = data.page
      this.totalPages = data.total_pages
      this.filteredUsers = this.users;
    },
    filterUsers(){
      this.filteredUsers = this.users.filter(user => {
        let yes = false;
        yes = yes || user.first_name.toLowerCase().includes(this.searchTerm.toLowerCase());
        yes = yes || user.last_name.toLowerCase().includes(this.searchTerm.toLowerCase());
        yes = yes || user.email.toLowerCase().includes(this.searchTerm.toLowerCase());
        return yes;
      })
    }
  },
  watch:{
    searchTerm(){
      this.filterUsers();
    }
  },
  data(){
    return {
      users : [],
      filteredUsers: [],
      searchTerm : '',
      currentPage : null,
      totalPages: 1
    }
  }
}
</script>