<template>
  <div>


    <div class="input-box">
      <a>Skills</a>
      <hr>
      <div v-for="ski in allSkills" v-if="showSki==true">
        <button id="addMore" v-on:click="removeSkills(ski)">{{ski}}</button>
      </div>
      <hr v-if="showSki==true">

      <div class="column">
        <input type="text" v-model="Skill" placeholder="Enter Skills(e.g. Time Management)" required value=Skill
               v-on:input="skillsInput"/>
        <button v-on:click='addSkills' v-if="skillButton==true"><img src="/add.png" width="20" height="30"></button>
      </div>
      <div v-if="showSkill==true">
        <ul class="roles-list">
          <li v-for="ski in resultSkills" v-on:click="selectSkills(ski)">{{ ski }}</li>
        </ul>
      </div>
      <div class="column">
        <a id="feedback" v-html="feed"></a>
      </div>
      <div class="column">
        <button id="formButton" v-on:click="prevPage">← Previous</button>
        <button id="formButton" v-on:click="nextPage">{{ this.buttonText }} →</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Skills",
  props: ["skill", "skills","transferHistorys"],
  data() {
    return {
      showSkill: false,
      allSkills: [],
      Skill: '',
      showSki:false,
      resultSkills: [],
      skillButton: false,
      buttonText: 'Next',
      feed: '',

    }
  },
  created() {
    if (this.skill.length > 0) {
      this.allSkills = this.skill;
      this.showSki=true;
    }
  },
  methods: {

    removeSkills(ski){
      this.allSkills=this.allSkills.filter(web=>web!==ski);
if(this.allSkills.length<1){
  this.showSki=false;
}
    },

    selectSkills(ski) {
      let found = false;
      this.allSkills.forEach(skii => {
        if (skii == ski) {
          found = true;
        }

      });
      if (found == false) {
        this.allSkills.push(ski);

      }
      this.Skill = "";
      this.showSkill = false;
      this.skillButton = false;
      this.showSki=true;


    },
    skillsInput() {

      if (this.Skill !== undefined && this.Skill !== "") {
        this.resultSkills = [];
        this.skills.forEach(ski => {
          this.resultSkills.push(ski);
        });
        this.resultSkills = this.resultSkills.filter((rol) => rol.toLowerCase().includes(this.Skill.toLowerCase()));
        if (this.resultSkills.length > 0) {
          this.showSkill = true;
          this.skillButton = false;
        } else {
          this.skillButton = true
        }

      } else {
        this.resultSkills = [];
        this.showSkill = false;
        this.skillButton = false;
      }
    },


    addSkills() {
      if (this.Skill !== undefined && this.Skill !== "") {

        let found = false;
        this.allSkills.forEach(skii => {
          if (skii == this.Skill) {
            found = true;
          }

        });
        if (found == false) {
          this.allSkills.push(this.Skill);

        }
        this.Skill = "";
        this.showSkill = false;
        this.skillButton = false;
        this.showSki=true;

      }


    },


    prevPage: function () {
      this.$emit('changeSkill', this.allSkills);
      this.$emit('changeTitle', 'Work Experience');
      this.$emit('changePage', 'Work');


    },
    nextPage: function () {
      this.$emit('changeSkill', this.allSkills);
      if(this.transferHistorys.length>0){

        this.$emit('changeTitle', 'Transfer Module Selection');
        this.$emit('changePage', 'TransferModules');
      }
      else{
      this.$emit('changeTitle', 'Module Selection');
      this.$emit('changePage', 'Modules');
      }

    },

  },
}
</script>

