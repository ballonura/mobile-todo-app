<template>
  <div class="container h-screen overflow-x-hidden">
    <div class="flex flex-col sm:hidden h-full">
      <my-header style="flex-basis: 75px;"></my-header>
      <Nuxt class="flex-grow" />
    </div>
    <div class="hidden sm:block">Not yet implemented... :(</div>
  </div>
</template>

<script>
import MyHeader from "@/components/my-header";

export default {};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700;1,900&display=swap");

* {
  font-family: "Source Sans Pro", sans-serif;
}
</style>
