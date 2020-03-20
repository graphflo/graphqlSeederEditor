<template>
  <div id="app" class="light">
    <app-nav />
    <main>
      <nuxt/>
    </main>
  </div>
</template>
<script>
    import 'normalize.css'
    import AppNav from '~/components/AppNav.vue'
/*    import AppItem from './components/AppItem.vue'
    import AppDarkmode from './components/AppDarkmode.vue'
    import AppLightmode from './components/AppLightmode.vue'*/
    export default {
        components: {
            AppNav
        },

    }
</script>
<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Space+Mono|Source+Sans+Pro:400,600');

  #__nuxt, #__layout {
    width: 100%;
  }
  body {
    display: flex;
    font-family: 'Source Sans Pro', 'Helvetica Neue', Arial, sans-serif;
    font-size: 16px;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    max-width: 100vw;
    min-height: 100vh;
  }

  #app {
    width: 100vw;
    min-height: 100vh;
    color: #34495e;
    background-color: #212121;
  }

  main {
    /*width: 900px;*/
    margin: 0 auto;
  }

  h1,
  h2,
  h3 {
    text-align: center;
  }

  @media only screen and (max-width: 600px) {
    main {
      width: 100vw;
    }
  }

  $ltgrey: #212121;
  $dkgrey: #34495e;
  .light {
    nav {
      border-bottom: 1px solid $dkgrey;
      background: #42b983;
    }
    .icons svg path {
      fill: #34495e;
    }
    .block {
      background-color: #fff;
      border: 1px solid  $ltgrey;
      border-bottom-color: #34495e;
      button {
        border: 1px solid  $ltgrey;
        &:focus {
          outline: 1px dashed  $ltgrey;
        }
      }
    }
    .tgl-btn {
      background: #34495e;
      color: #fff;
      font-weight: bold;
      border: 4px solid #34495e;
    }
    .note,
    .findoutmore {
      margin-left: 20px;
      background-color: #fff;
      border: 1px solid  $ltgrey;
      border-bottom-color: #ccc;
    }
  }
</style>

<style scoped>
  p {
    text-align: center;
  }
</style>
