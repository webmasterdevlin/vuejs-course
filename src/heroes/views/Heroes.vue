<template>
    <section>
        <h1>Heroes Works!</h1>
        <Form :text="'SAVE'" :obj="heroForm" @handleSubmit="onSave"  />
        <ul v-if="heroes">
            <li v-for="hero in heroes" :key="hero.id" >
                <span>{{`${hero.firstName} ${hero.lastName}`}}</span> <button @click="removeHero(hero.id)">DELETE</button>
            </li>
        </ul>
    </section>
</template>

<script>
  import { deleteById, get, post } from '../../shared/api-call'
  import Form from '../../shared/components/Form'

  export default {
    name: 'Heroes',
    components: {Form},
    data() {
      return {
        heroes: [],
        heroForm : {
          firstName: "",
          lastName: "",
          house: "",
          knownAs: ""
        }
      }
    },
    methods: {
      async removeHero(id) {
        try {
          await deleteById('heroes', id);
          this.heroes = this.heroes.filter(h => h.id !== id);
        } catch (e) {
          alert(e.message)
        }
      },
      async onSave() {
        try {
          const {data} = await post('heroes', this.heroForm);
          this.heroes = [...this.heroes, data]
        } catch (e) {
          alert(e.message);
        }
      },
      async onUpdate() {
        try {
          // TODO:
        } catch (e) {
          alert(e.message)
        }
      }
    },
    async mounted () {
        const {data} = await get('heroes');
        this.heroes = data;
    }
  }
</script>

<style scoped>

</style>
