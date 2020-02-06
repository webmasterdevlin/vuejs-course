<template>
    <form @submit.prevent="onSubmit">
        <input type="text" placeholder="First Name" v-model="obj.firstName" />
        <input type="text" placeholder="Last Name" v-model="obj.lastName" />
        <input type="text" placeholder="House" v-model="obj.house" />
        <input type="text" placeholder="Known As" v-model="obj.knownAs" />
        <button type="submit" style="background-color: greenyellow">{{text}}</button>
    </form>
</template>

<script>
  export default {
    name: 'Form',
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
        this.$emit('handleSubmit');
      }
    }
  }
</script>

<style scoped>

</style>
