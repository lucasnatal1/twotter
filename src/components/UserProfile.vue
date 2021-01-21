<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
        
        <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
          <label for="newTwoot"><strong> New Twoot </strong></label>
          <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>

          <div class="user-profile__create-twoot-type">
            <label for="newTwootType"><strong> Type: </strong></label>
            <select id="newTwootType" v-model="selectedTwootType">
              <option :value="option.value" v-for="(option, index) in twootTypes" :key='index'>
                {{ option.name }}
              </option>
            </select>
          </div>

          <button>
            Twoot!
          </button>
        </form>
      </div>
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
  name: "App",
  components: { TwootItem },
  data() {
    return {
      newTwootContent: ' ',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: "draft", name: "Draft" },
        { value: "instant", name: "Instant Twoot" },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_LuanaCSM",
        firstName: "Luana",
        lastName: "Marques",
        email: "luanacsmarques@gmail.com",
        isAdmin: true,
        twoots: [
          { id: 1, content: "Twotter is Brabo!" },
          { id: 2, content: "Dou uns pega em Natal" },
        ],
      },
    };
  },
  watch: {
    //whatches a datapoint, and executes something when the datapoint changes
    followers(newFollowersCount, oldFollowersCount) {
      if (oldFollowersCount < newFollowersCount)
        console.log(`${this.user.username} has gained a follower!`);
    },
  },
  computed: {
    //calculates what you want it to
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    },
  },
  methods: {
    //functions to call yourself
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      console.log(`Favorite twoot #${id}`);
    },
    createNewTwoot() {
      if(this.newTwootContent && this.selectedTwootType !== 'draft') {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent
        })
        this.newTwootContent = '';
      }
    },
  },
  mounted() {
    //called when a component is loaded the first time (lifecycle)
    this.followUser();
  },
};
</script>

<style>
.user-profile {
  display: grid;
  /*grid-template-columns: 1fr 3fr;*/
  grid-template-columns: 1fr 3fr;
  padding: 50px 5%;
  width: 90%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

.user-profile__admin-badge {
  background: rebeccapurple;
  color: white;
  border-radius: 5px;
  padding: 3px 10px;
  font-weight: bold;
  margin: 3px;
  margin-right: auto;
}

.user-profile__create-twoot {
  border-top: 1px solid #dfe3e8;
  padding-top: 20px;
  display: flex;
  flex-direction: column;
}

.user-profile__twoots-wrapper {
  display: grid;
  grid-gap: 10px;
}
</style>
