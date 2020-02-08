<template>
  <section>
    <div v-if="editingTracker === '0'">
      <Form :text="'SAVE'" :obj="heroForm" @handleSubmit="onSave" />
    </div>
    <h3 v-if="isLoading">loading..</h3>
    <section v-else class="mx-auto">
      <div v-if="heroes.length > 0" >
        <div v-for="hero in heroes" :key="hero.id" class="card my-4">
          <div class="card-body">
          <Form
            :text="'UPDATE'"
            :obj="hero"
            @handleSubmit="onUpdate(hero)"
            v-if="editingTracker === hero.id"
          />
            <div v-else class="mb-4">
              <h5 class="card-title mb-3">{{
                `${hero.firstName} ${hero.lastName} is ${hero.knownAs}`
                }}</h5>
              <p class="card-text">{{ hero.house }}</p>
            </div>
          <div>
            <b-button
              v-if="editingTracker === hero.id"
              @click="() => editingTracker = '0'"
              variant="outline-primary"
              class="mr-2"
            >
              CANCEL
            </b-button>
            <b-button
              v-else
              @click="() => (editingTracker = hero.id)"
              variant="primary"
              class="mr-2"
            >
              EDIT
            </b-button>

            <b-button @click="goDetail(hero.id)" variant="info" class="mr-2">DETAIL</b-button>
            <b-button
              @click="removeHero(hero.id)"
              variant="danger"
            >
              DELETE
            </b-button>
          </div>
        </div>
        </div>
      </div>
      <h3 v-else>List is empty</h3>
    </section>
  </section>
</template>

<script>
import { deleteById, get, post, put } from "../../shared/api-call";
import Form from "../../shared/components/Form";

export default {
  name: "Heroes",
  components: { Form },
  data: () => ({
    heroes: [],
    heroForm: {
      id: "",
      firstName: "",
      lastName: "",
      house: "",
      knownAs: ""
    },
    editingTracker: "0",
    isLoading: false
  }),
  methods: {
    async removeHero(id) {
      // this.isLoading = true;
      try {
        await deleteById("heroes", id);
        this.heroes = this.heroes.filter(h => h.id !== id);
      } catch (e) {
        alert(e.message);
      }
      // this.isLoading = false;
    },
    async onSave() {
      this.isLoading = true;
      try {
        const { data } = await post("heroes", this.heroForm);
        this.heroes = [...this.heroes, data];
      } catch (e) {
        alert(e.message);
      }
      this.isLoading = false;
    },
    async onUpdate(values) {
      if (!values.firstName || !values.lastName) {
        alert("First Name and Last Name are required.");
        return;
      }
      this.isLoading = true;
      try {
        await put("heroes", values);
        const index = this.heroes.findIndex(h => h.id === values.id);
        this.heroes[index] = values;
        this.editingTracker = "0";
      } catch (e) {
        alert(e.message);
      }
      this.isLoading = false;
    },
    goDetail(id) {
      this.$router.push({ name: "hero-detail", params: { id } });
    }
  },
  async mounted() {
    this.isLoading = true;
    try {
      const { data } = await get("heroes");
      this.heroes = data;
    } catch (e) {
      alert(e.message);
    }
    this.isLoading = false;
  }
};
</script>

<style scoped></style>
