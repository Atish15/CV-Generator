<template>
  <div>

    <a id="feedbacks">(This is an optional section where you have to add other sections)</a>
    <div v-if="showExtras==true">
      <div v-if="this.communityExperience.length>0">
        <div class="input-box">
        <h4>Community Experience</h4>
        <div v-for="dat in communityExperience">
          <div class="input-box">

          <a>Experience</a>
          <input type="text" :disabled="true"  :value=dat.name />

          <div v-if="dat.description!==' '">
            <a>Description</a>
            <input type="text" :disabled="true"  :value=dat.description />
          </div>

              <button v-on:click='deleteCommunity(dat)' width="15" height="15" >Remove<img src="/delete.png" width="15" height="15"></button>
          </div>
        </div>
      </div>
      </div>

      <div v-if="this.achievements.length>0">
        <div class="input-box">
        <h4>Achievements</h4>
        <div v-for="dat in achievements">
          <div class="input-box">

              <a>Achievement</a>
              <input type="text" :disabled="true"  :value=dat.name />

            <div  v-if="dat.description!==' '">
              <a>Description</a>
              <input type="text" :disabled="true"  :value=dat.description />
            </div>
          </div>
              <button v-on:click='deleteAchievement(dat)' width="15" height="15" >Remove<img src="/delete.png" width="15" height="15"></button>

          </div>
        </div>
      </div>
      <div v-if="hobby.length>0">
        <div class="input-box">
        <h4>Hobbies</h4>
        <div v-for="hob in hobby">
          <div class="column">
          <input type="text" :disabled="true"  :value=hob /> <button  v-on:click="removeHobby(hob)" width="15" height="15" >Remove<img src="/delete.png" width="15" height="15"></button>
        </div>
        </div>
      </div>
      </div>
    </div>
    <hr>
    <button id="addMores" v-on:click="addExtras('Community Experience')">Community Experience</button>
    <button id="addMores" v-on:click="addExtras('Personal Statement')">Personal Statement</button>
    <button id="addMores" v-on:click="addExtras('Achievement')">Achievements</button>
    <button id="addMores" v-on:click="addExtras('Objective')">Objective</button>
    <button id="addMores" v-on:click="addExtras('Hobbies')">Hobbies</button>
    <div v-if="showExtras==true">
      <hr>
<div v-if="showCommunity==true">
  <div class="input-box">
    <a>Community Experience</a>
    <input type="text" v-model="communityExperienceName" placeholder="Enter Community Experience" required value=communityExperienceName />
  </div>
  <div class="input-box">
    <a>Description</a>
    <input type="text" v-model="communityExperienceDescription" placeholder="Describe about the experience" required value=communityExperienceDescription />
  </div>
  <button id="moreButton" v-on:click="addCommunityExperience">Add</button>

</div>

      <div class="showstate" v-if="showStatement==true">
        <textarea type="text"  id="skillsText" v-model="personalStatement" placeholder="Enter Personal Statement "   value=personalStatement />
        <button id="moreButton" v-on:click="addStatement">Add</button>
      </div>
      <div class="showstate" v-if="showObjective==true">
        <textarea type="text"  id="skillsText" v-model="objective" placeholder="Enter Your Objective "   value=objective />
        <button id="moreButton" v-on:click="addObjective">Add</button>
      </div>
      <div v-if="showAchievement==true">
        <div class="input-box">
          <a>Achievement</a>
          <input type="text" v-model="achievementName" placeholder="Enter Achievement" required value=achievementName />
        </div>
        <div class="input-box">
          <a>Achievement Description</a>
          <input type="text" v-model="achievementDescription" placeholder="Describe about the achievement" required value=achievementDescription />
        </div>
        <button id="moreButton" v-on:click="addAchievement">Add</button>

      </div>
      <div v-if="showHobbies==true">
        <h4>Enter Hobbies</h4>
        <div class="column">
          <input type="text" v-model="hobbyText" placeholder="Enter Hobbies" required value=hobbyText
                 v-on:input="hobbiesInput"/>
          <button v-on:click='addHobby' v-if="hobbyButton==true"><img src="/add.png" width="20" height="30"></button>
        </div>
        <div v-if="showHobby==true">
          <ul class="roles-list">
            <li v-for="hob in resultHobby" v-on:click="selectHobby(hob)">{{ hob }}</li>
          </ul>
        </div>

      </div>

    </div>
    <div class="column">
      <button id="formButton" v-on:click="prevPage">← Previous</button>
      <button id="formButton" v-on:click="nextPage">{{ this.buttonText }} →</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Extras",
  props:["hobbies","extraSkill"],
  data() {
    return {
      buttonText:'Finish',
      showExtras:false,
      showHobby:false,
      extraSkills:[],
      communityExperience:[],
      hobbyButton:false,
      personalStatement:'',
      achievements:[],
      resultHobby:[],
      showAchievement:false,
      objective:'',
      hobby:[],
      hobbyText:'',
      showCommunity:false,
      showObjective:false,
      showHobbies:false,
      showStatement:false,
      communityExperienceName:'',
      communityExperienceDescription:'',
      achievementName:'',
      achievementDescription:'',
    }
    },
  created:async  function (){
if(this.extraSkill.length>0){
  this.extraSkills=this.extraSkill;
  this.communityExperience=this.extraSkills[0];
  this.personalStatement=this.extraSkills[1];
  this.achievements=this.extraSkills[2];
  this.objective=this.extraSkills[3];
  this.hobby=this.extraSkills[4];
  this.showExtras=true;

}

              },
  methods:{


removeHobby(hob){
  this.hobby=this.hobby.filter(web=>web!==hob);
},
    hobbiesInput(){
      if (this.hobbyText !== undefined && this.hobbyText !== "") {
        this.resultHobby = [];
        this.hobbies.forEach(ski => {
          this.resultHobby.push(ski);
        });
        this.resultHobby = this.resultHobby.filter((rol) => rol.toLowerCase().includes(this.hobbyText.toLowerCase()));
        if (this.resultHobby.length > 0) {
          this.showHobby = true;
          this.hobbyButton = false;
        }
        else {
          this.resultHobby=[];
          this.showHobby = false;
          this.hobbyButton = true
        }

      } else {
        this.resultHobby= [];
        this.showHobby = false;
        this.hobbyButton = false;
      }

    },
    addHobby(){
      if (this.hobbyText !== undefined && this.hobbyText !== "") {

        let found = false;
        this.hobby.forEach(skii => {
          if (skii == this.hobbyText) {
            found = true;
          }

        });
        if (found == false) {
          this.hobby.push(this.hobbyText);

        }
        this.hobbyText = "";
        this.showHobby = false;
        this.hobbyButton = false;


      }



    },
    selectHobby(hob){
      let found = false;
      this.hobby.forEach(ski => {
        if (ski == hob) {
          found = true;
        }

      });
      if (found == false) {
        this.hobby.push(hob);

      }
      this.hobbyText = "";
      this.hobbyButton = false;
      this.showHobby=false;

    },
    deleteCommunity(dat){
      this.communityExperience=this.communityExperience.filter(web=>web!==dat);

    },

addExtras(feature){
this.showExtras=true;
if(feature=="Community Experience"){
  if(this.showCommunity==false){
this.showCommunity=true;
this.showStatement=false;
this.showAchievement=false;
this.showObjective=false;
this.showHobbies=false;
  }
  else{
    this.showCommunity=false;
  }

}
  if(feature=="Personal Statement"){
    if(this.showStatement==false){
      this.showStatement=true;
      this.showCommunity=false;
      this.showAchievement=false;
      this.showObjective=false;
      this.showHobbies=false;
    }
    else{
      this.showStatement=false;
    }

  }
  if(feature=="Achievement"){
    if(this.showAchievement==false){
      this.showAchievement=true;
      this.showCommunity=false;
      this.showStatement=false;
      this.showObjective=false;
      this.showHobbies=false;
    }
    else{
      this.showAchievement=false;
    }

  }
  if(feature=="Objective"){
    if(this.showObjective==false){
      this.showAchievement=false;
      this.showCommunity=false;
      this.showStatement=false;
      this.showObjective=true;
      this.showHobbies=false;
    }
    else{
      this.showObjective=false;
    }

  }
  if(feature=="Hobbies"){
    if(this.showHobbies==false){
      this.showAchievement=false;
      this.showCommunity=false;
      this.showStatement=false;
      this.showObjective=false;
      this.showHobbies=true;
    }
    else{
      this.showHobbies=false;
    }

  }
},
    addAchievement(){
      if(this.achievementName!==undefined && this.achievementName!==""){
        if(this.achievementDescription==""){
          this.achievementDescription=" ";
        }
        this.achievements.push({"name":this.achievementName,"description":this.achievementDescription});
        this.achievementDescription="";
        this.achievementName="";
        this.showAchievement=false;

      }
      if((this.achievementName==undefined || this.achievementName=="")&& (this.achievementDescription==undefined || this.achievementDescription=="")){
        this.showachievement=false;
      }


    },
    deleteAchievement(dat){
      this.achievements=this.achievements.filter(web=>web!==dat);

    },
    addCommunityExperience(){
if(this.communityExperienceName!==undefined && this.communityExperienceName!==""){
  if(this.communityExperienceDescription==""){
    this.communityExperienceDescription=" ";
  }
  this.communityExperience.push({"name":this.communityExperienceName,"description":this.communityExperienceDescription});
  this.communityExperienceDescription="";
  this.communityExperienceName="";
  this.showCommunity=false;

}
      if((this.communityExperienceName==undefined || this.communityExperienceName=="")&& (this.communityExperienceDescription==undefined || this.communityExperienceDescription=="")){
            this.showCommunity=false;
      }
    },
    addStatement(){
      this.showStatement=false;
    },
    addObjective(){
      this.showObjective=false;
    },

    prevPage: function () {
  this.extraSkills=[];
  this.extraSkills.push(this.communityExperience);
  this.extraSkills.push(this.personalStatement);
  this.extraSkills.push(this.achievements);
  this.extraSkills.push(this.objective);
  this.extraSkills.push(this.hobby);
      this.$emit('changeextraSkill', this.extraSkills);
      this.$emit('changeTitle', 'Middlesex University Modules');
      this.$emit('changePage', 'Modules');


    },
    nextPage: function () {
      this.extraSkills=[];
      this.extraSkills.push(this.communityExperience);
      this.extraSkills.push(this.personalStatement);
      this.extraSkills.push(this.achievements);
      this.extraSkills.push(this.objective);
      this.extraSkills.push(this.hobby);
      this.$emit('changeextraSkill', this.extraSkills);
        this.$emit('changeTitle', 'Final');
        this.$emit('changePage', 'Final');


    },
  },
}
</script>
