<template>
  <div>
    <nuxt />
  </div>
</template>

<script>

export default {
  components: {},
  computed: {}

};
</script>

<style lang="scss">
  // GLOBAL

  body {
    color: black;

    h1, h2, h3, p , a {
      color: black;
    }
  }

  .logo_wrap {
    background: black;
    padding: 50px 0;
    text-align: center;
    position: relative;

      @media(max-width:820px){
        width: 270px;
      }
  }

  .logo {
    max-width: 50%;
    height: auto;
    margin: 0 auto;
  }

  h2.heading {
    font-size: 18px;
    font-weight: bold;
  }

  // ELEMENTS

  .color_swatch {
    height: 20px;
    width: 20px;
    display: inline-block;
    border: 1px solid #e7e7e7;
    border-radius: 50px;
  }

  .uk-slidenav:hover {
    color: white;
    background: black;
}

  

</style>
