<template>
  <div>
    <section class="container">
      <sfeir-card
        v-for="person in people"
        :person="person"
        :key="person.id"
        @delete="deletePerson"
      ></sfeir-card>
    </section>
    <md-dialog ref="dialog">
      <md-dialog-title>Contact informations</md-dialog-title>
      <md-dialog-content>
        <sfeir-form @save="addPerson" @cancel="hideDialog"></sfeir-form>
      </md-dialog-content>
    </md-dialog>
    <md-button
      class="md-fab md-fab-bottom-right md-primary"
      @click="showDialog"
    >
      <md-icon>add</md-icon>
    </md-button>
  </div>
</template>

<script>
import peopleService from "../services/PeopleService.js";
import CardPanel from "../components/CardPanel.vue";
import Form from "../components/Form.vue";

export default {
  components: {
    "sfeir-card": CardPanel,
    "sfeir-form": Form,
  },
  data() {
    return {
      people: [],
    };
  },
  beforeRouteEnter(route, redirect, next) {
    peopleService
      .fetch()
      .then((people) =>
        next((vm) => {
          vm.people = people;
        })
      )
      .catch(console.log.bind(console));
  },
  methods: {
    deletePerson: function (person) {
      peopleService
        .delete(person.id)
        .then((people) => {
          this.people = people;
        })
        .catch(console.log);
    },
    addPerson: function (person) {
      peopleService
        .create(person)
        .then((person) => {
          this.people.push(person);
          this.hideDialog();
        })
        .catch(console.log);
    },
    showDialog() {
      this.$refs["dialog"].open();
      this.showModal = true;
    },
    hideDialog() {
      this.$refs["dialog"].close();
      this.showModal = false;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

.card-panel {
  padding: 10px;
  margin: 10px !important;
  min-width: 500px;
}

.card-panel:hover {
  box-shadow: 0 11px 15px -7px rgba(0, 0, 0, 0.2),
    0 24px 38px 3px rgba(0, 0, 0, 0.14), 0 9px 46px 8px rgba(0, 0, 0, 0.12);
}

.md-card .md-subhead {
  opacity: 1;
}

.picture {
  border-radius: 50%;
  width: 100%;
  height: 100%;
}

.username {
  color: #337ab7;
  font-size: 24px;
  font-weight: 400;
}

.lastname {
  text-transform: uppercase;
}

.subtitle {
  color: rgba(0, 0, 0, 0.54);
}

.people-data {
  margin-top: 20px;
}

.item {
  color: #337ab7;
}

.people-data a {
  padding-left: 10px;
}

.icon {
  color: lightslategrey;
  width: 24px;
  height: 24px;
}

.label {
  font-weight: bold;
}

.skills {
  padding: 10px;
  margin: 10px;
  background-color: #fafafa;
}

.skills h3 {
  font-size: 24px;
  font-weight: normal;
  line-height: 1.1;
}

.skills a {
  background-color: white;
  color: #000;
  margin: 5px;
}

.links {
  text-align: center;
}

.links img {
  padding: 0 5px;
}
</style>
