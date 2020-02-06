<template>
  <section>
    <h1>Heroes Works!</h1>
    <div v-if="editingTracker === '0'">
      <Form :text="'SAVE'" :obj="heroForm" @handleSubmit="onSave" />
    </div>
    <ul v-if="heroes">
      <li v-for="hero in heroes" :key="hero.id">
        <Form
          :text="'UPDATE'"
          :obj="hero"
          @handleSubmit="onUpdate(hero)"
          v-if="editingTracker === hero.id"
        />
        <span v-else>{{
          `${hero.firstName} ${hero.lastName} is ${hero.knownAs}`
        }}</span>

        <div>
            <button v-if="editingTracker === hero.id" @click="() => editingTracker = '0'" style="background-color: lightcyan">CANCEL</button>
          <button v-else @click="() => (editingTracker = hero.id)" style="background-color: lightcyan">EDIT</button>

            <button @click="goDetail(hero.id)">DETAIL</button>
            <button @click="removeHero(hero.id)" style="background-color: deeppink">DELETE</button>
        </div>
      </li>
    </ul>
  </section>
</template>

<script>
  import { deleteById, get, post, put } from '../../shared/api-call'
import Form from "../../shared/components/Form";

export default {
  name: "Heroes",
  components: { Form },
  data:() => ({
      heroes: [],
      heroForm: {
        id: "",
        firstName: "",
        lastName: "",
        house: "",
        knownAs: ""
      },
      editingTracker: "0"
  }),
  methods: {
    async removeHero(id) {
      try {
        await deleteById("heroes", id);
        this.heroes = this.heroes.filter(h => h.id !== id);
      } catch (e) {
        alert(e.message);
      }
    },
    async onSave() {
      try {
        const { data } = await post("heroes", this.heroForm);
        this.heroes = [...this.heroes, data];
      } catch (e) {
        alert(e.message);
      }
    },
    async onUpdate(values) {
      if (!values.firstName || !values.lastName) {
        alert("First Name and Last Name are required.");
        return;
      }

      try {
        await put('heroes', values);
        const index = this.heroes.findIndex(h => h.id === values.id)
        this.heroes[index] = values;
        this.editingTracker = '0';
      } catch (e) {
        alert(e.message);
      }
    },
    goDetail(id) {
      this.$router.push({ name: "hero-detail", params: { id } });
    }
  },
  async mounted() {
    const { data } = await get("heroes");
    this.heroes = data;
  }
};
</script>

<style scoped></style>
