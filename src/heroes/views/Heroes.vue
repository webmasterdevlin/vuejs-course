<template>
    <section>
        <h1>Heroes Works!</h1>
        <ul v-if="heroes">
            <li v-for="hero in heroes" :key="hero.id" >
                <span>{{`${hero.firstName} ${hero.lastName}`}}</span> <button @click="removeHero(hero.id)">DELETE</button>
            </li>
        </ul>
    </section>
</template>

<script>
  import { deleteById, get } from '../../shared/api-call'

  export default {
    name: 'Heroes',
    data() {
      return {
        heroes: []
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
