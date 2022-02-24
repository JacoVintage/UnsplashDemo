<template>
    <div class="container">
    <button class="menu_toggle uk-hidden@m uk-button uk-button-default uk-margin-small-right" type="button" uk-toggle="target: #offcanvas-push"><span uk-icon="icon: menu"></span></button>
     

    <div id="offcanvas-push" uk-offcanvas="mode: push; overlay: true">
    <div class="uk-offcanvas-bar">
     
        <div class="button-wrapper">
          <div class="logo_wrap">
            <button class="uk-offcanvas-close" type="button" uk-close></button>
          <img class="logo" src="/logo.svg">
          </div>
          <ul>
            <li v-for="(item, index) in topics" :key="index"> <button class="btn" :class="{'button-active': item.title === liTopic}" @click="searchUnsplash('searchTopic', item.title)">{{item.title}}</button> </li>
          </ul>
          <ul class="color_swatch--list">
                <button class="color_swatch" v-for="(item, index) in colours" :key="index" :style="{ background: item.color }"  @click="searchUnsplash('searchColor', liTopic , item.color)"></button>
          </ul>
          <form class="uk-search uk-search-default">
              
                  <button @click="searchUnsplash('searchTopic', searchTopic , pageNumber)" class="uk-search-icon-flip" uk-search-icon></button>
                  <input class="uk-search-input" type="search" placeholder="Search" v-model="searchTopic">
              </form>
              
        </div>
      </div>
  </div>

    <div class="button-wrapper uk-visible@m">
      <div class="logo_wrap">
      <img class="logo" src="/logo.svg">
      </div>
      <ul>
        <li v-for="(item, index) in topics" :key="index"> <button class="btn" :class="{'button-active': item.title === liTopic}" @click="searchUnsplash('searchTopic', item.title)">{{item.title}}</button> </li>
      </ul>
      <ul class="color_swatch--list" v-if="liTopic">
            <button class="color_swatch" :class="{'swatch-active': item.color === liColor}" v-for="(item, index) in colours" :key="index" :style="{ background: item.color }"  @click="searchUnsplash('searchColor', topic , item.color)"></button>
      </ul>
      <form class="uk-search uk-search-default">
           
              <button @click="searchUnsplash('searchTopic', searchTopic , pageNumber)" class="uk-search-icon-flip" uk-search-icon></button>
              <input class="uk-search-input" type="search" placeholder="Search" v-model="searchTopic">
          </form>
          
    </div>
    <div class="display">
 
    
    <div class="display__blocks">
      <div class="display__blocks--scroll" :style="{left : scrollPos + '%'}" uk-lightbox="animation: slide">
        <div class="img_wrap" v-for="(image, i) in images" :key="i">
           <a :href="image.urls.regular + '&h=800'" :data-caption="image.alt_description"  type="iframe"  :style="{ backgroundImage: `url(${image.urls.small})` }">
              <!-- <img :src="image.urls.small" :alt="image.alt_description" > -->
            </a>
        </div>
        <div class="img_wrap final">
          <button class="uk-button uk-button-default button-next" @click="morePages(liTopic, pageNumber, color)"> Load More </button>
        </div>  
      </div>

       <a class="uk-position-center-left uk-position-small uk-hidden-hover uk-visible@m" v-if="scrollPos <= -35" @click="previousPage()" uk-slidenav-previous uk-slider-item="previous"></a>
    <a class="uk-position-center-right uk-position-small uk-hidden-hover uk-visible@m"  @click="nextPage()" uk-slidenav-next uk-slider-item="next"></a>
    </div>  

    </div>     
     
    </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";


export default {
name: 'Unsplash',
components: {
    Stack,
    StackItem
},
data() {
    return {
         unsplash:'https://api.unsplash.com/search/photos',
         queryString: '',
         images: [],
         topics: [],
         imgBig : false,
         colours: [
           {color : 'black'},
           {color : 'white'},
           {color : 'yellow'},
           {color : 'orange'},
           {color : 'red'},
           {color : 'purple'},
           {color : 'green'},
           {color : 'teal'},
           {color : 'blue'}
         ],
         isActive: false,
         liTopic : '',
         liColor: '',
         pageNumber : 9,
         topic: '',
         color: '',
         searchTopic: '',
         scrollPos: 0

    }
},
methods: {
     searchUnsplash(type , topic, color) {

      if (type == 'searchTopic'){
          this.scrollPos = 0;
          this.liTopic = topic;
          this.topic = topic;
          this.pageNumber = 1;
          this.color = '';
          this.unsplash = `https://api.unsplash.com/search/photos?query=${topic}&per_page=28`

      }

      if (type == 'searchColor'){
          this.scrollPos = 0;
          this.color = color;
          this.liColor = color;
          this.unsplash = `https://api.unsplash.com/search/photos?query=${topic}&per_page=28&color=${color}`
      }
      

      // AXIOS CALL
      axios
      .get(
        this.unsplash,
        {
          headers: {
            Authorization:
              "Client-ID ArnXX3bqjssGVe0JJSwj5VAKibi1zIZrAzKvMenJ2gA",
              "Accept-Version": "v1"
          }
        }
      )
      .then(response => {
        this.images = response.data.results;
      })
      .catch(() => {
        this.images = [];
      });

     
     
    },
    
 
    nextPage(){
      if (this.scrollPos > -245 ){
        this.scrollPos = this.scrollPos - 35;
      }
      
    },
    previousPage(){
      if (this.scrollPos < 0 ){
        this.scrollPos = this.scrollPos + 35;
      }
    },

    morePages(topic , pagination , color){
        this.pageNumber++;
        this.scrollPos = 0;

        if (color == ''){
          this.unsplash = `https://api.unsplash.com/search/photos?query=${topic}&page=${pagination}&per_page=28`
        } else {
          this.unsplash = `https://api.unsplash.com/search/photos?query=${topic}&page=${pagination}&per_page=28&color=${color}`
        }

        axios
        .get(
          this.unsplash,
          {
            headers: {
              Authorization:
                "Client-ID ArnXX3bqjssGVe0JJSwj5VAKibi1zIZrAzKvMenJ2gA",
                "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    },


    loadTopics() {
      axios
        .get(
          `https://api.unsplash.com/topics`,
          {
            headers: {
              Authorization:
                "Client-ID ArnXX3bqjssGVe0JJSwj5VAKibi1zIZrAzKvMenJ2gA",
                "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.topics = response.data;
        })
        .catch(() => {
          this.topics = [];
        });
    },
    loadScreen() {
      this.images = [];
      axios
        .get(
          this.unsplash + `?query=random&per_page=28`,
          {
            headers: {
              Authorization:
                "Client-ID ArnXX3bqjssGVe0JJSwj5VAKibi1zIZrAzKvMenJ2gA",
                "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    },
    toggleBig() {
      this.imgBig = !this.imgBig;
      // some code to filter users
    }
},
computed: {},
mounted() {
   this.loadScreen();
   this.loadTopics();
   
}

 
}

</script>

<style lang="scss">
 
  // COMPONENT

  // NAVIGATION

  .container {
     .menu_toggle {
        position: absolute;
        right: 0;
        z-index: 99;
        background: black;
        color: white;
        border: none;
        padding: 10px 25px;
        margin: 0 !important;
      }


    .button-wrapper {
      width: 15%;
      display: inline-block;
      float: left;
      position: fixed;

      @media(max-width:820px){
        width: 100%;
      }

     
      ul {
        padding: 0 15px;

        @media(max-width: 820px){
              width: 200px;
        }

        li {
          list-style-type: none;

          button {
            background: transparent;
            width: 100%;
            border: none;
            padding: 15px 0;
            text-align: left;
            border-bottom: 1px solid #e7e7e7;
            padding-left: 5px;
            border-left: 5px solid white;
            position: relative;
            

            &:hover {
              cursor: pointer;
              
              
              &:after {
                 display: block;
                  content: '';
                  height: 5px;
                  width: 5px;
                  top: -2px;
                  left: -2px;
                  background: grey;
                  position: absolute;
              }
            }
          }

          button.btn.button-active {
            font-weight: bold;

            &:before {
              display: block;
              content: '';
              height: 5px;
              width: 5px;
              top: -2px;
              left: -2px;
              background: black;
              position: absolute;
            }
            
          }
        }
      }

      ul.color_swatch--list {
        padding: 10px 25px;

        button {
          margin: 0 10px 10px 0;
          display: inline-block;

          &:hover {
            cursor: pointer;
          };
        }

        button.swatch-active {
          border: 2px solid black;
        }
      }

      .uk-search-default {
          width: 80%;
          margin-left: 25px;

           @media(max-width: 820px){
              width: 200px;
              background: black;
              border: none;
        }
      }


    }

    .uk-offcanvas-bar {
      background-color: white;
      padding: 0;
    }

    // DISPLAY AREA

    .display {
      width: calc(85% - 30px);
      display: inline-block;
      float: left;
      position: relative;
      left: 15%;
      background: #ededed;
      height: 100vh;
      padding: 0 15px;

      @media(max-width:820px){
        width: 100%;
        left: 0;
        padding: 0;
      }

      .display__blocks {
        position: relative;
        overflow: hidden;

        .display__blocks--scroll {
          grid-template-rows: 1fr 1fr 1fr; 
          display: grid;
          grid-auto-flow: column;
          grid-auto-columns: minmax(33.3%,1fr);
          // overflow-x: auto;
          position: relative;
          grid-gap: 1%;

          @media(max-width: 820px){
            grid-template-columns: 1fr 1fr 1fr; 
            grid-template-rows: 1fr 1fr 1fr; 
            gap: 0px 0px; 
            grid-template-areas: 
              ". . . ."
              ". . . ."; 
              grid-auto-columns: minmax(50%,1fr);
              overflow-x: auto;
          }
        }

        .img_wrap {
    
          position: relative;

          a {
            display: block;
            width: 100%;
            height: 33.25vh;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
          }

          img {
            width: 100%;
            height: auto;
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
          }

          img.img_big {
            
          }
        }

        .img_wrap.final {
          background-color: black;

          button {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
            height: 75px;
            background: white;
          }
        }
      }

      .display__header {
        height: 75px;
        position: relative;
        text-align: center;
        position: relative;

        h2.heading {
          top: 25px;
          position: absolute;
          left: 0;
          right: 0;
          margin: 0 auto;
        }

        .button-next {
          position: absolute;
          right: 15px;
          top: 15px;
        }

         .button-previous {
          position: absolute;
          right: 125px;
          top: 15px;
        }

        .color_swatch {
          position: absolute;
          top: 15px;
          left: 15px;
        }

     
      }
      

   




    }
  }

</style>