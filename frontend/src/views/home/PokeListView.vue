<template>
  <v-container class="fill-height mxw">
    <v-row justify="center" align="center">
      <v-col cols="6">
        <v-card>
          <v-card-title class="headline pb-5 text-center" @click="print()"> Pokemons </v-card-title>
          <div class="d-flex maxw justify-center">
            <v-col v-for="item in items" :key="item.id" cols="12">
              <Poke :pokemon="item" @click="goDuel"/>
            </v-col>
          </div>
        </v-card>
      </v-col>

      <!-- <v-col cols="12">
        <Poke-form :form-label="'Nova Tarefa'" @new-task="addNewTask" />
      </v-col> -->
    </v-row>
  </v-container>
</template>

<script>
import { useAppStore } from "@/stores/appStore"
import TasksApi from "@/api/tasks.api.js"
import mock from "@/api/api-mock"
import Poke from "@/components/Poke.vue"
import PokeForm from "@/components/PokeForm.vue"

export default {
  name: "TasksList",
  components: { Poke, PokeForm },
  setup() {
    const appStore = useAppStore()
    return { appStore }
  },
  data() {
    return {
      loading: false,
      items: [],
      pokemons: [{
        id: 51,
        name: 'pikachu',
        type: 'repampago'
      },{},{}]
    }
  },
  mounted() {
    this.getPokes()
    // this.getPokes_mock()

  },
  methods: {
    goDuel() {
      this.$router.push({ name: "poke-duel" })
    },
    getPokes() {

      this.loading = true
      TasksApi.getPokemon().then((data) => {
        this.items = JSON.parse(data)
        this.loading = false
        console.log(this.items)
      })
    },
    async getPokes_mock() {
      this.loading = true
      this.items = await mock.mocked_api.getPokes()
      this.loading = false
      console.log(this.items)
    },
    addNewTask(task) {
      this.loading = true
      TasksApi.addNewTask(task.title).then((task) => {
        this.appStore.showSnackbar(`Nova tarefa adicionada #${task.id}`)
        this.getTasks()
        this.loading = false
        console.log("oi")
      })
    },
  },
}
</script>

<style scoped>
.done {
  text-decoration: line-through;
}

.v-col-12 {
  max-width: 33% !important;
}

.mxw {
  flex-direction: column;
}

.maxw {
  flex-flow: wrap;
}
</style>
