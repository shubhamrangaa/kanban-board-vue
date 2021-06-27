<template>
  <div>
    <h1>hellow from vue</h1>
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "firstComp",
  props: {
    msg: String,
  },
};
</script>
