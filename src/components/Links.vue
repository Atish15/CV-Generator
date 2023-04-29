<template>

  <div>
    <h3>{{this.miniTitle}}</h3>
    <hr>
    <div v-if="activeLinkedin==true">
      <div class="input-box">
        <a>Linkedin Site <button><img src="/delete.png" width="20" height="20" v-on:click="deleteLinkedin"/></button> </a>

        <input type="text" v-model="linkedinLink" placeholder="Please add a link e.g(linkedin.com/user)" required value=linkedinLink />
      </div>
      <hr>
    </div>
    <div v-if="activeWebsites==true">
      <div class="input-box" v-for="links in websitesLink">
        <a>Websites Site <button><img src="/delete.png" width="20" height="20" v-on:click="deleteWebsites(links.count)"/></button> </a>
        <input type="text" v-model="links.name" placeholder="What is your website type(e.g. personal,blog,game)" required value=links.name />
        <input type="text" v-model="links.link" placeholder="Please add your website link)" required value=links.link />
        <hr>
      </div>
    </div>
    <button id="addMore" v-if="activeLinkedin==false" v-on:click="addLinkedin">Linkedin</button>
    <button id="addMore"  v-on:click="addWebsites">Websites</button>


  <div class="column">
    <a id="feedback" v-html="feed"></a>
  </div>
    <div class="column">
      <button id="formButton" v-on:click="prevPage">← Previous</button>
      <button id="formButton" v-on:click="nextPage">{{this.buttonText}} →</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Links",
  props:['linkedinLinks','websitesLinks'],
  data() {
    return {
      miniTitle:'',
      buttonText:'Next',
      linkedinLink:'',
      websitesLink:[],
      activeLinkedin:false,
      activeWebsites:false,
      feed:'',

    }
  },
  created() {
    if(this.linkedinLinks!==undefined && this.linkedinLinks!==""){
         this.linkedinLink=this.linkedinLinks
      this.activeLinkedin=true;
    }

    if(this.websitesLinks.length!==undefined && this.websitesLinks.length>0){
      this.websitesLink=this.websitesLinks;
      this.activeWebsites=true;
    }
  },
  methods: {
    addWebsites: function () {
      this.activeWebsites = true;
      if (this.websitesLink.length < 1) {
        this.websitesLink.push({name: '', link: '', count: 0});
      } else {
        let tm = this.websitesLink.length - 1;
        this.websitesLink.push({name: '', link: '', count: this.websitesLink[tm].count + 1});
      }

    },
    addLinkedin: function () {
      this.activeLinkedin = true;

    },
    deleteLinkedin: function () {
      this.activeLinkedin = false;
      this.linkedinLink = "";
    },
    deleteWebsites: function (param) {
      this.websitesLink = this.websitesLink.filter(web => web.count !== param);
    },
    prevPage: function () {
      if(this.linkedinLink!==undefined && this.linkedinLink!==""){
        this.$emit('changeLinkedinLink', this.linkedinLink);
      }
      if(this.websitesLink.length>0){
        this.$emit('changeWebsitesLinks', this.websitesLink);
      }
      this.$emit('changeTitle', 'Education and Websites link');
      this.$emit('changePage', 'UniversityEducation');
    },
    nextPage: function () {
      if(this.linkedinLink!==undefined && this.linkedinLink!==""){
        this.$emit('changeLinkedinLink', this.linkedinLink);
      }
      if(this.websitesLink.length>0){
        this.$emit('changeWebsitesLinks', this.websitesLink);
      }
      this.$emit('changeTitle', 'Work Experience');
      this.$emit('changePage', 'Work');
    },
  }


}
</script>

<style scoped>

</style>