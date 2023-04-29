

<template>

  <div id="app">

    <section class="container">
      <button id="download" v-if="currentComponent=='Final' && show==false" v-on:click="printCV">Download</button>
      <header v-if="currentComponent!=='Final'">{{this.title}}</header>
      <div class="form">
        <component :is="currentComponent" :transferYears="transferYears" :extraSkill="extraSkill" :hobbies="hobbies"
                   :mdxSkills="mdxSkills" :transferSkills="transferSkills" :transferHistorys="transferHistorys"
                   :moduleDetails="moduleDetails" :skill="skill" :workHistory="workHistory" :jobQualifications="jobQualifications"
                   :skills="skills" :mdxHistorys="mdxHistorys" :personalDetail="personalDetail"
                   :educationHistorys="educationHistorys" :mdxHistroys="mdxHistorys" :linkedinLinks="linkedinLinks"
                   :websitesLinks="websitesLinks"
                    @changePersonal="changePersonal"  @changeTransferYears="changeTransferYears"
                   @changeextraSkill="changeextraSkill" @changemdxSkill="changemdxSkill"
                   @changeTransferSkill="changeTransferSkill"
                   @changeTransfer="changeTransfer" @changeModule="changeModule"  @changeSkill="changeSkill"
                   @changeWorkHistory="changeWorkHistory"  @changeLinkedinLink="changeLinkedinLink"
                   @changeWebsitesLinks="changeWebsitesLinks"   @changeMdx="changeMdx"  @changeEducation="changeEducation"
                   @changeSummary="changeSummary" @changePage="changePage" @changeTitle="changeTitle" :summary="summary">

        </component>
      </div>
    </section>


  </div>
</template>

<script>
import PersonalDetails from './components/PersonalDetails.vue';
import Summary from './components/Summary.vue'
import Education from "./components/Education.vue";
import UniversityEducation from "@/components/UniversityEducation.vue";
import Work from "./components/Work.vue";
import Links from "./components/Links.vue";
import Skill from "./components/Skills.vue";
import Modules from "./components/Modules.vue";
import TransferModules from "@/components/TransferModules.vue";
import Final from "@/components/Final.vue";
import Extras from "@/components/Extras.vue";
import Skills from "./Json/skils.json";
import Hobbies from "./Json/hobbies.json";
import Qualifications from "./Json/qualification.json";
export default {
  name: "App",
  components: {PersonalDetails,Summary,Education,UniversityEducation,Links,Work,Skill,Modules,TransferModules,Extras,Final},
  data() {
    return {
      educationHistorys:[],
      mdxHistorys:[],
      extraSkill:[],
      transferHistorys:[],
      show:false,
      websitesLinks:[],
      transferSkills:[],
      mdxSkills:[],
      skills:Skills.skills,
      jobQualifications:Qualifications.Data,
      linkedinLinks:'',
      title:'Personal Details',
      personalDetail:{name:'',last:'',email:'',number:0,address1:'',address2:'',country:'Country',nationality:'Nationality',city:'',region:'',code:'',birth:'',Cmale:false,Cfemale:false,CNmale:false},
      currentComponent:'PersonalDetails',
      summary:'',
      transferYears:'',
      moduleDetails:[],
      workHistory:[],
      skill:[],
      hobbies:Hobbies.hobbies,
    }

  },

  methods: {
    changePage: function (page) {
            this.currentComponent=page;
    },
    changePersonal: function (page) {
      this.personalDetail=page;
    },
    changeTitle:function (titles){
      this.title=titles;
    },
    changeSummary:function (summ){
      this.summary=summ;
    },
    changeModule:function (mod){
      this.moduleDetails=mod;
    },
    changeEducation:function (edu){
      this.educationHistorys=edu;
    },
    changeMdx:function (edu){
      this.mdxHistorys=edu;
    },

    changeTransfer:function (edu){
      this. transferHistorys=edu;
    },
    changeTransferSkill:function (ski){
      this. transferSkills=[];
      this. transferSkills=ski;
    },
    changeextraSkill:function (ski){
      this.extraSkill=ski;
    },
    changemdxSkill:function (ski){
      this. mdxSkills=ski;
    },

    changeLinkedinLink:function (link){
      this.linkedinLinks=link;
    },
    changeWebsitesLinks:function (link){
      this.websitesLinks=link;
    },
    changeWorkHistory:function (link){

      this.workHistory=link;
    },
    changeTransferYears:function (ski){
   this.transferYears=ski;
    },
    changeSkill:function (link){

      this.skill=link;
    },
    printCV(){
      this.show=true;
      let element = document.getElementById('app');
    //  html2pdf().from(element).save();
      let opt =
          {
            margin:       1,
            filename:     'CV.pdf',
            image:        { type: 'jpeg', quality: 0.98 },
            html2canvas:  { scale: 2 },
            jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
          };

      // New Promise-based usage:
     html2pdf().set(opt).from(element).save();


    },
  },
}

</script>

