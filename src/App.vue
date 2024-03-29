<template>
  <div id="app">
    <WindowSize v-if="checkTypeOfEnv === '-test build-'" />
    <HeaderUSGS ref="headerUSGS" />
    <InternetExplorerPage v-if="isInternetExplorer" />
    <!-- an empty string in this case means the 'prod' version of the application   -->
    <router-view
      v-if="!isInternetExplorer"
    />
    <PreFooterCodeLinks v-if="!isInternetExplorer" />
    <FooterUSGS />
  </div>
</template>

<script>
    import WindowSize from "./components/WindowSize";
    import HeaderUSGS from './components/HeaderUSGS';
    import { isMobile } from 'mobile-device-detect';
    export default {
        name: 'App',
        components: {
            WindowSize,
            HeaderUSGS,
            InternetExplorerPage: () => import( /* webpackPrefetch: true */ /*webpackChunkName: "internet-explorer-page"*/ "./components/InternetExplorerPage"),
            PreFooterCodeLinks: () => import( /* webpackPrefetch: true */ /*webpackChunkName: "pre-footer-links-code"*/ "./components/PreFooterCodeLinks"),
            FooterUSGS: () => import( /* webpackPrefetch: true */ /*webpackChunkName: "usgs-footer"*/ "./components/FooterUSGS") // Have Webpack put the footer in a separate chunk so we can load it conditionally (with a v-if) if we desire
        },
        data() {
            return {
                isInternetExplorer: false,
                title: process.env.VUE_APP_TITLE,
                publicPath: process.env.BASE_URL, // this is need for the data files in the public folder
                mobileView: isMobile,
            }
        },
        computed: {
          checkTypeOfEnv() {
              return process.env.VUE_APP_TIER
          },
          windowWidth: function() {
            return this.$store.state.windowWidth
          },
          windowHeight: function () {
              return this.$store.state.windowHeight
          }
        },
        mounted(){
          console.log(this.$store.state.windowHeight)
        },
        created() {
            // We do not support for Internet Explorer. This tests if the browser used is IE.
            this.$browserDetect.isIE ? this.isInternetExplorer = true : this.isInternetExplorer = false;
            // Add window size tracking by adding a listener and a way to store the values in the Vuex state
            window.addEventListener('resize', this.handleResize);
            this.handleResize();
        },
        destroyed() {
            window.removeEventListener('resize', this.handleResize);
        },
        methods:{
          handleResize() {
                this.$store.commit('recordWindowWidth', window.innerWidth);
                this.$store.commit('recordWindowHeight', window.innerHeight);
            },
        }
    }
</script>

<style lang="scss">
// Fonts
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@200;300;400;500;600;700;800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Edu+SA+Beginner:wght@400;500;600;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Rubik+Moonrocks&display=swap');
$scriptFont: 'Edu SA Beginner', cursive;
$SourceSans: 'Source Sans Pro', sans-serif;
$textcolor: white;
$linkColor:rgb(242, 255, 172);
$fontCurly: 'Rubik Moonrocks', cursive;

#app {
  width: 100%;
  //height: calc(100vh + 85.7px); //85.7 is the height of the USGS header
}

// Type
html,
body {
      height: 100%;
      //background-color: rgb(233, 245, 238);
      background-color:#2b4b52;
      margin: 0;
      padding: 0;
      line-height: 1.2;
      font-size: 18px;
      font-weight: 400;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      width: 100%;
      @media screen and (max-width: 600px) {
        font-size: 16px;
      }
  }
h1{
  font-size: 4.5em;
  font-weight: 600;
  font-family: $SourceSans;
  line-height: 1;
  text-align: left;
  color: $textcolor;
  @media screen and (max-width: 600px) {
    font-size: 4em;
  }
}
h2{
  font-weight: 500;
  text-align: left;
  font-family: $scriptFont;
  font-size: 3em;
  margin-top: 5px;
  line-height: 1.2;
    color: $textcolor;
  @media screen and (max-width: 600px) {
    font-size: 2.75em;
  }
}
h3{
  font-size: 1.75em;
  padding-top: .25em;
  font-family: $SourceSans;
  font-weight: 700;
    color: $textcolor;
  @media screen and (max-width: 600px) {
      font-size: 1.5em;
  }  
}
h4{
  font-size: 1.5em;
  padding-top: .25em;
  font-family: $SourceSans;
  font-weight: 700;
    color: $textcolor;
  @media screen and (max-width: 600px) {
      font-size: 1.5em;
  }  
}
p, text {
  padding: 1em 0 0 0; 
  font-family: $SourceSans;
  color: $textcolor;
}
 .text-content {
    min-width: 300px;
    max-width: 700px;
    margin: 0 auto;
    padding: 10px;   
    @media screen and (max-width: 600px) {
        padding: 10px;
    }  
  }
    .flex-container {
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    justify-content: space-evenly;
    align-content: space-around;
    max-width: 100%;
    margin: auto;
    @media screen and (max-width: 600px) {
        max-width: 100%;
    }
  }
  .flex-item {
    padding: 20px;
    min-width: 400px;
    flex: 0 0 auto;
    align-self: center;
  }
  @media (max-width: 600px) {
    .flex-container {
      flex-direction: column;
    }
    .flex-item {
      flex: none;
      padding: 0 0 1em 0;
      height: 100%;
    }
  }
  .figure-content {
    border: 1px white;
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    justify-content: space-evenly;
    align-content: space-around;
    max-width: 100%;
    // padding: 0 100px;
    margin: auto;
    @media screen and (max-width: 600px) {
        padding: 0px; 
    }
  }

  // Link Styling
a:link { 
  text-decoration: underline;
  //font-weight: bold;
  color: $linkColor;
  padding: 5px 0;
} 
a:visited { 
  color: $linkColor; 
} 
a:hover {
  text-decoration:none;
  text-shadow: 0 0 2px lightGrey;  
  cursor: pointer;
} 
a:focus { 
    text-shadow: 0 0 2px lightGrey;
}
a:active {
  padding-top: 3px;
}
</style>
