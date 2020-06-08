<template>
  <div class="column_wrapper">
    <div>
      <header>Resolved:</header>
      <ul>
      <li v-for="error in resolved" :key="error.index">
        <h6>'{{ error.code }}' - {{ error.text }}</h6>
        <button v-on:click="pushFromResolvedToUnresolved(error)">Unresolved</button>
      </li>
      </ul>
       
    </div>
    <div>
      <header>Unresolved:</header>
      <ul>
      <li v-for="error in unresolved" :key="error.index">
        <h6>'{{ error.code }}' - {{ error.text }}</h6>
        <button v-on:click="pushFromUnresolvedToResolved(error)">Resolved</button>
      </li>
      </ul>
    </div>
    <div>
      <header>Backlog:</header>
      <ul>
        
      <li v-for="error in backlog" :key="error.index">
        <h6>'{{ error.code }}' - {{ error.text }}</h6>
        <button v-on:click="pushFromBacklogToUnresolved(error)">Unresolved</button>
      </li>
      </ul>
    </div>
  </div>
</template>

<script>
// const EMPTY_STATE = 'emptyState'; 

// import Vue from 'vue'
// import Vuex from 'vuex'


// Vue.use(Vuex)

// const store = new Vuex.Store({
//   state: {
//     // myVal: null,
//     index: null,
//     prv_status: null,
//     code: null,
//     current_status: null,
//   },
//   mutations: {
//     updateState(state, data) {
//       // alert(index, code, prv_status);
//       // this.replaceState({ myval: null });
//       state.index= data[0];
//       console.log(data[0]);
//       state.code = data[1];
//       console.log(data[1]);
//       state.prv_status = data[2];
//       console.log(data[2]);
//       state.current_status= data[3];      
//     }
//   }
// });



export default {
  async asyncData({ $axios }) {
    try {
      let { resolved, unresolved, backlog } = await $axios.$get(
        "http://localhost:8000/get_lists"
      );
      return {
        resolved,
        unresolved,
        backlog
      };
    } catch (error) {
      console.log(
        `Couldn't get error lists:\n${error}\nDid you start the API?`
      );
      console.log(
        "HINT: You can comment out thes full `asyncData` method and work with mocked data for UI/UX development, if you want to."
      );
    }
  },

  data() {
    return {
      resolved: [],
      unresolved: [],
      backlog: [],
      // done: [],
      // store: store,
    };
  },
//  created() {
//     this.store.subscribe(mutation => {
//       if (mutation.type !== EMPTY_STATE) {
//         this.done.push(mutation);
//       }
//     });
//   },
methods: {
  pushFromResolvedToUnresolved(error) {
    this.unresolved.push(error)
    // this.store.commit("updateState", [this.resolved.indexOf(error), error.code, this.resolved, this.unresolved]);
    this.resolved.splice(this.resolved.indexOf(error), 1)
  },
  pushFromUnresolvedToResolved(error) {
    this.resolved.push(error)
    // this.store.commit("updateState", [this.unresolved.indexOf(error), error.code, this.unresolved, this.resolved]);
    this.unresolved.splice(this.unresolved.indexOf(error), 1)
  },
  pushFromBacklogToUnresolved(error) {
    this.unresolved.push(error)
    this.backlog.splice(this.backlog.indexOf(error), 1)
    // this.store.commit("updateState",[this.backlog.indexOf(error), error.code, this.backlog, this.unresolved]);
  },
//  undo(error) {
  //  console.log(store.state.prv_status)
  //  store.state.current_status[store.state.prv_status.indexOf(error)] = error;
  //  store.state.current_status.splice(store.state.current_status.indexOf(error), 1)
      // });
    // } 
}
 
};
</script>


<style scoped>
body {
  padding: 2em;
  font-family: sans-serif;
}

.column_wrapper {
  max-height: 200px;
  display: flex;
  flex-flow: column wrap;
  padding:0.5em;
}

.column_wrapper header{
  display: block;
  border-radius: 3px 3px 0 0;
  background: green;
  padding: 5px;
  color: white;
  font-size: 1.7em;
}

.column_wrapper div {
  padding:1em;
}

.column_wrapper ul { 
  list-style: none;
  margin: 0;
  padding: 0 1em 1em;
}

.column_wrapper ul li {
  padding: .25em;
  border: 0.5px solid gray;
  margin: .5em 0;
  border-radius: 2px;
}

button {
  padding: .3em;
  border: 0.2em;
  background-color:green;
  color: white;
  font-size: 0.8em;
  transition-duration: 0.4s;
}

button:hover {
  background-color: #4CAF50; /* Green */
  box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);
  color: white;
}
</style>