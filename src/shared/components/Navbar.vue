<template>
        <section class="mb-4">
            <b-navbar type="dark" variant="info">
               <b-navbar-brand>
                   <router-link to="/" style="text-decoration: none; color: white">Home</router-link>
               </b-navbar-brand>
               <b-navbar-nav>
                   <b-nav-item>
                       <router-link to="/heroes" style="text-decoration: none; color: white">Heroes</router-link>
                   </b-nav-item>
                    <b-nav-item>
                        <router-link to="/" style="text-decoration: none; color: white">Villains</router-link>
                    </b-nav-item>
               </b-navbar-nav>
            </b-navbar>
        </section>
</template>

<script>
  export default {
    name: 'Navbar'
  }
</script>

<style scoped>

</style>
