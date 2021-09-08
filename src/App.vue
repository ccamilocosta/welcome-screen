<template>

  <div id="app">
  
    <h1 class="site-title">{{title}}</h1>
    <span class="site-description">{{currentDate}}</span>

    <ul class ="entry-list">
      <li
         v-for ="entry in filteredEntries" 
         :key="entry.id"
         class="entry-item"
      >
      
        <span class ="entry-daytime">{{entry [0]}} Uhr, {{entry[1].replaceAll("/",".")}}</span>
        <h3 class="entry-title">{{entry [2]}}</h3>
        <span class="entry-description">{{entry [3]}}</span>
      </li>
    </ul>

    <footer class="footer">
      <img 
        src="./assets/STZH_SEB_Logo.png"
        alt="Stadt Zürich Soziale Betriebe und Einrichtungen Logo"
      >

      <img
        src="./assets/Opportunity.png"
        alt="opportunity Logo"
      >

      <img
        src="./assets/SAG_Logo_De.png"
        alt="Stiftung Arbeitsgestaltung Logo"
      >
    </footer>
      
  </div>
  
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data(){
    return {
      title: "Welcome to Opportunity",
      currentDate:"",
      gsheet_url:
      "https://sheets.googleapis.com/v4/spreadsheets/14wl5n-gRB3qmgHAZjZuYAESOgcstqv3duOaneFhUWJQ/values:batchGet?ranges=A1%3AE100&valueRenderOption=FORMATTED_VALUE&key=AIzaSyAUQvNndCJ1AIzKUdbogJm03OkKXRLrMKI",
      entries: [],

    };
  },
  computed: { // // computed properties are like data properties, but with a method combined, it gets executed automatically, instead of calling a function explicitly
    filteredEntries(){
      return [...this.entries].slice(1);
    }
  },
    
 
  methods: {
    getData() {
      axios.get(this.gsheet_url).then((response) => {
       this.entries = response.data.valueRanges[0].values;
        console.log(response);
      }); 
  
    },
    
    updateCurrentDate () {
      let today = new Date();
      const date =  `${today.getDate()}.${today.getMonth() + 1}.${today.getFullYear()}`;
      this.currentDate = date;
    },
    refreshData() {
      this.getData();
      this.updateCurrentDate();
    },
  },
  mounted() {
    this.refreshData();
    setInterval(() => {
      this.refreshData();
    }, 180000)
   },
}; 
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap');

body {
  background: #E5E5E5;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;
}

.site.title {
  font-weight: 900;
  font-size: 62px;
  margin: 80px 0 20px 0;

}
  
.site-description {
  font-weight: 500;
  font-size: 62px;
  line-height: 36px;
  color: #9aa7b1;
  margin: 0;
}

.entry-list {
  padding-left: 0;
}

.entry-title  {
  font-weight:900;
  color: #EB5E00;
}

.entry-item {
  padding:35px 40px;
  margin: 40px 0;
  background:#0f05a0;
  font-size: 28px;
  line-height: 1.3;
  list-style: none;
}

.entry-daytime {
  color: #EB5E00;
  font-weight: 900;
}

.entry-title {
  font-size: inherit;
  margin: 0;
  color: #ffbfab;
  font-weight: 900;
}

.entry-description {
  color: #ffbfab;
}

.footer {
  display:flex-wrap;
  justify-content:space-between;
  background: #FFFFFF;
  padding: 40px;
  box-sizing: border-box;
  position: fixed;
  bottom:0;
  left: 0;
  width: 100%;
  
}

.footer img {
  height: 50px;
  
}
@media screen and (min-width: 480px) {
  #leftsidebar {width: 200px; float: left;}
  #main {margin-left: 216px;}
}


</style>
