<template>
  <id-not-found v-if="isNotaId"></id-not-found>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
    </ul>
  </section>
</template>

<script>
import UserItem from "../users/UserItem.vue";

export default {
  components: {
    UserItem,
  },
  inject: ["teams", "users"],
  data() {
    return {
      teamName: "",
      members: [],
      isNotaId: false,
    };
  },
  props: ["teamId"],
  methods: {
    logwithMember(teamId) {
      const selectedTeam = this.teams.find((team) => team.id === teamId);
      if (selectedTeam === undefined) {
        this.isNotaId = true;
      } else {
        const members = selectedTeam.members;
        const selectedMembers = [];
        for (const member of members) {
          const selectedUser = this.users.find((user) => user.id === member);
          selectedMembers.push(selectedUser);
        }
        this.members = selectedMembers;
        this.teamName = selectedTeam.name;
        console.log(this.members);
      }
    },
  },
  created() {
    this.logwithMember(this.teamId);
  },
  watch: {
    $route(newId) {
      this.logwithMember(newId);
    },
  },
  provide() {
    return{
      isNotaId: this.isNotaId
    }
  }
};
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
