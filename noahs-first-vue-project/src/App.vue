<script setup>
import { ref, reactive } from "vue";

const reactiveMessage = reactive({
  message: "Hi World!",
});

const count = ref(0)
</script>

<template>
  <body>
    <section>
      <h1>
        {{ reactiveMessage.message }}
      </h1>
    </section>
    <section>
      <p>{{ count }}</p>
    </section>
  </body>
</template>

<style scoped>

</style>
