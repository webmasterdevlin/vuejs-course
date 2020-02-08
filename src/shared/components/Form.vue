<template>
  <b-form @submit="onSubmit" class="mb-4">
    <b-form-group label="First Name" label-for="">
      <b-form-input id="firstName" v-model="obj.firstName" />
    </b-form-group>
    <b-form-group label="Last Name" label-for="">
        <b-form-input id="lastName" v-model="obj.lastName" />
    </b-form-group>
    <b-form-group label="House" label-for="">
        <b-form-input id="house" v-model="obj.house" /> </b-form-group
    ><b-form-group label="Known As" label-for="">
      <b-form-input id="knownAs" v-model="obj.knownAs" />
    </b-form-group>
    <b-button type="submit" variant="success">{{ text }}</b-button>
  </b-form>
</template>

<script>
export default {
  name: "Form",
  props: {
    text: {
      type: String,
      default: () => String
    },
    obj: {
      type: Object,
      default: () => {}
    }
  },
  methods: {
    onSubmit() {
      this.$emit("handleSubmit");
    }
  }
};
</script>

<style scoped></style>
