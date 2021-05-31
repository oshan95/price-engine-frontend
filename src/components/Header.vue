<template>
  <section class="test">
    <div>
      <h4>
        <router-link to="/">Price List</router-link> |
        <router-link to="/calculator">Calculate Amount</router-link>
      </h4>
    </div>
  </section>
</template>

<style lang="scss" scoped>
.test {
  border-bottom: double;

  a {
    color: blue;
  }
}
</style>