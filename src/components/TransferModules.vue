<template>

  <div>
    <h3>{{ this.miniTitle }}</h3>
    <hr>
    <div v-for="ski in allSkills" v-if="showSki==true">
      <button id="addMore" v-on:click="removeSkills(ski)">{{ski}}</button>
    </div>
    <hr v-if="showSki==true">
    <h4 v-if="showBox==false">Year {{this.yearText}} Skills</h4>
    <div class="input-box" v-if="showBox==true">
      <label>Year</label>
      <select v-model="selectYear" v-on:change="yearChange" value=selectYear>
        <option>Year 1</option>
      </select>
    </div>

    <div class="column" v-if="showBox==false">
      <div class="input-box">
      <input type="text" v-model="skill" placeholder="Enter skills learned" required value=skill/>
      </div>
      <button v-on:click='addSkills' width="20" height="30"><img src="/add.png" width="20" height="30"></button>
    </div>
    <button id="formButton" v-on:click="nextYear">Next Year</button>

    <div class="column">
      <a id="feedback" v-html="feed"></a>
    </div>
    <div class="column">
      <button id="formButton" v-on:click="prevPage">← Previous</button>
      <button id="formButton" v-on:click="nextPage">{{ this.buttonText }} →</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TransferModules",
  props: ["transferHistorys", "transferSkills"],
  data() {
    return {
      moduleDetail: [],
      feed: '',
      skill:'',
      buttonText: "Next",
      miniTitle: 'Transfer University Modules Detail',
      checkTransfer: false,
      selectYear: 0,
      showBox:true,
      yearText:1,
      allSkills:[],
      showSki:false,

    }
  },
  created() {
    let today = new Date();
    if (this.transferHistorys.length !== undefined && this.transferHistorys.length > 0) {
      this.checkTransfer = true;
    }
    if (this.transferSkills.length > 0) {
      this.allSkills = this.transferSkills[0].skills;
      this.yearText=this.transferSkills[0].year;
      this.showSki=true;
      this.showBox=false;

    }


  },
  methods: {
    nextYear(){
      if(this.yearText<4){
    this.yearText+=1;
        if (this.skill !== undefined && this.skill !== "") {

          let found = false;
          this.allSkills.forEach(skii => {
            if (skii == this.skill) {
              found = true;
            }

          });
          if (found == false) {
            this.allSkills.push(this.skill);
            this.showSki=true;

          }
          this.skill = "";


        }

      }
    },
    removeSkills(ski){
      this.allSkills=this.allSkills.filter(web=>web!==ski);
      if(this.allSkills.length<1){
        this.showSki=false;
      }
    },
    yearChange() {

   this.showBox=false;

    },



    addSkills() {
      if (this.skill !== undefined && this.skill !== "") {

        let found = false;
        this.allSkills.forEach(skii => {
          if (skii == this.skill) {
            found = true;
          }

        });
        if (found == false) {
          this.allSkills.push(this.skill);
          this.showSki=true;

        }
        this.skill = "";


      }


    },


    prevPage: function () {
      let final=[{"year":this.yearText,"skills":this.allSkills}]
      this.$emit('changeTransferSkill', final);
      this.$emit('changeTitle', 'Skills');
      this.$emit('changePage', 'Skill');


    },
    nextPage: function () {

      let final=[{"year":this.yearText,"skills":this.allSkills}]
      this.$emit('changeTransferSkill', final);
      this.$emit('changeTitle', 'Middlesex University Modules');
      this.$emit('changePage', 'Modules');

    },


  },


}

</script>

<style scoped>

</style>