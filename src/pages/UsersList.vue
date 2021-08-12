<template lang="pug">
button(@click="confirmInput") Confirmar
button(@click="saveChanges") Salvar mudanças
ul
	user-item(v-for="user in users" :key="user.id" :name="user.fullName" :role="user.role")
</template>

<script>
import UserItem from '../components/users/UserItem.vue';

export default {
  components: {
    UserItem,
  },
  inject: ['users'],
  data() {
    return { changesSaved: false };
  },
  methods: {
    confirmInput() {
      // do something
      this.$router.push('/teams');
    },
    saveChanges() {
      this.changesSaved = true;
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log('UsersList Cmp beforeRouteEnter');
    console.log(to, from);
    next();
  },
  beforeRouteLeave(to, from, next) {
    console.log('UsersList Cmp beforeRouteLeave');
    console.log(to, from);
    
    if (this.changesSaved) {
      next();
    } else {
      const userWantsToLeave = confirm('Tem certeza? Você tem mudanças não salvas');
      next(userWantsToLeave);
    }
  },
  unmounted() {
    console.log('unmounted');
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 20rem;
  padding: 0;
}
</style>