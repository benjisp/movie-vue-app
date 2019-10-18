<template>
  <body>
    <div class="home">
      <h2>New actor</h2>
      <div>
        First Name:
        <input type="text" v-model="newActorFirstName" />
        Last Name:
        <input type="text" v-model="newActorLastName" />
        Gender:
        <input type="text" v-model="newActorGender" />
        Age:
        <input type="text" v-model="newActorAge" />
        <button v-on:click="createActor()">Create Actor</button>
      </div>
      <h1>All Actors</h1>
      Search by name:
      <input v-model="nameFilter" list="names" />
      <datalist id="names">
        <option v-for="actor in actors">{{ actor.first_name }} {{ actor.last_name }}</option>
      </datalist>
      <div v-for="actor in filterBy(actors, nameFilter, 'first_name')">
        <h2>Name: {{ actor.first_name }} {{ actor.last_name }}</h2>
        <button v-on:click="showActor(actor)">More Info</button>
        <div v-if="currentActor === actor">
          <h4>Gender: {{ actor.gender }}</h4>
          <h4>Age: {{ actor.age }}</h4>
          <div>
            First Name:
            <input type="text" v-model="actor.first_name" />
            Last Name:
            <input type="text" v-model="actor.last_name" />
            Gender:
            <input type="text" v-model="actor.gender" />
            Age:
            <input type="text" v-model="actor.age" />
            <button v-on:click="updateActor(actor)">Update Actor</button>
            <button v-on:click="destroyActor(actor)">Destroy Actor</button>
          </div>
        </div>
      </div>
    </div>
  </body>
</template>

<style></style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      actors: [],
      currentActor: {},
      newActorFirstName: "",
      newActorLastName: "",
      newActorGender: "",
      newActorAge: "",
      nameFilter: ""
    };
  },
  created: function() {
    axios.get("/api/actors").then(response => {
      this.actors = response.data;
    });
  },
  methods: {
    createActor: function() {
      var params = {
        first_name: this.newActorFirstName,
        last_name: this.newActorLastName,
        gender: this.newActorGender,
        age: this.newActorAge
      };
      axios.post("/api/actors", params).then(response => {
        this.actors.push(response.data);
        this.newActorFirstName = "";
        this.newActorLastName = "";
        this.newActorGender = "";
        this.newActorAge = "";
      });
    },
    showActor: function(actor) {
      if (this.currentActor === actor) {
        this.currentActor = {};
      } else {
        this.currentActor = actor;
      }
    },
    updateActor: function(actor) {
      var params = {
        first_name: actor.first_name,
        last_name: actor.last_name,
        gender: actor.gender,
        age: actor.age
      };
      axios.patch("/api/actors/" + actor.id, params).then(response => {
        this.currentActor = {};
      });
    },
    destroyActor: function(actor) {
      axios.delete("/api/actors/" + actor.id).then(response => {
        var index = this.actors.indexOf(actor);
        this.actors.splice(index, 1);
      });
    }
  }
};
</script>
