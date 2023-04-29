<template>
  <div>

<div v-if="mdxSkill.length>0">
  <div v-for="dat in mdxSkill">
    <hr>
  <a>Module: <a id="skillsText">{{dat.module}}</a> </a></br>
    <a>Grade: <a id="skillsText">{{dat.Grade}}</a> </a></br>
    <a>Skill Learned: <a id="skillsText">{{dat.skill}}</a> </a>
    <hr>
  </div>

</div>
    <div class="column">
      <a id="feedback" v-html="feedB"></a>
    </div>
    <div v-if="checkTransfer==true">
      <hr>
      <div class="input-box">
        <label>Level (Level in which you transferred in the University) </label>
        <select v-model="transferYear" v-on:change="selectLevel" value=transferYear>
          <option hidden>Select an option</option>
          <option :value=4>Level 4</option>
          <option :value=5>Level 5</option>
          <option :value=6>Level 6</option>
        </select>
      </div>
    </div>

    <div v-if="showDetails==true">
      <h3>{{ this.miniTitle }}</h3>

       <div v-if="showOptional==false">
         <div v-if="showModule==true">
         <div class="input-box">
           <label>Module:</label><a>{{data.code}}</a>
         </div>
         <div class="input-box">
           <label>Code:</label><a>{{data.name}}</a>
         </div>
         <div class="column">
         <div class="input-box">
           <label>Grade</label>
           <input type="text" v-model="marks" placeholder="Enter Point Grade(1-20)" required value=marks /><button v-on:click="skillsGenerate">Generate Skills</button>
         </div>
         </div>
         <div class="input-box" v-if="showSkills==true">
           <label>Skills:</label><a ><textarea type="text"  id="skillsText" v-model="dataSkills" placeholder="Enter Skills you learned or explain your project " required value=dataSkills /></a>
         </div>

       </div>


         <button id="moreButton" v-on:click="changePrev" v-if="firstModule==false">Previous Module</button>
         <button id="moreButton" v-on:click="changeModule" v-if="lastModule==false">Next Module</button>

       </div>
      <div v-if="showOptional==true">
        <h4>Please Select Optional Modules</h4>
        <div v-if="showSelected==true">
          <div v-for="opt in selectOptional">
          <button id="addMores" v-on:click="removeModule(opt)">{{opt}}</button>
        </div>
        </div>
    <div>

      <select v-model=optionalModules v-on:change="optionalSelect" value=optionalModules>
        <option v-for="opt in optionalData">{{opt}}</option>
      </select>
    </div>
      </div>

 </div>
    <button id="moreButton" v-on:click="editModules" v-if="showDetails==false && checkTransfer==false">Edit Modules</button>
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
  name: "Modules",
  props: ["mdxHistorys", "transferHistorys", "mdxSkills","transferYears"],
  data() {
    return {
      showOptional:false,
      firstModule:true,
      lastModule:false,
      moduleDetail: [],
      showSkills:false,
      feedB:'',
      showModule:true,
      mdxSkill: [],
      feed: '',
      buttonText: "Next",
      miniTitle: 'First Year Modules',
      checkTransfer: false,
      transferYear: '',
      optionalModules:'',
      tempCompulsory:[],
      showDetails: true,
      startYear: 1,
      tempDetails:[],
      data:{},
      dataSkills:'',
      marks:'',
      compulsory:[],
      optional:[],
      cCount:1,
      selectOptional:[],
      optionalData:[],
      showSelected:false,



    }
  },
  created: async function () {
    let corsesResult = await axios.get("https://cvbuildproject-env.eba-pzdzkfze.eu-west-2.elasticbeanstalk.com/modules");
    this.moduleDetail = corsesResult.data;

    if (this.transferHistorys.length > 0) {
      if (this.mdxSkills.length > 0) {
        this.checkTransfer = false;
        this.showDetails = false;
        this.mdxSkill=this.mdxSkills;
      }
      else{
        this.checkTransfer = true;
        this.showDetails = false;
      }

      this.transferYear=this.transferYears;
    }
    else{
      if (this.mdxSkills.length > 0) {
        this.mdxSkill = this.mdxSkills;
        this.showDetails = false;

      }
      else{
        this.showDetails=true;
        this.checkTransfer=false;
        this.modulesShow();
      }


    }



  },
  methods: {
    editModules(){
      this.cCount=4;
      this.startYear=3;
      this.showDetails=true;
      this.lastModule=false;
      this.firstModule=false;
      this.modulesShow();


    },
optionalSelect(){

  let diff=4-this.compulsory.length;

  if(diff>this.selectOptional.length){
    let found=false;
    this.selectOptional.forEach(dat=>{
      if(dat==this.optionalModules){
        found=true;
      }
    });
    if(found==false){
this.selectOptional.push(this.optionalModules);
      this.showSelected=true;
      this.feed="";
    }
    else{
      this.feed="You can't select same Module"
    }
this.optionalModules="";

if(diff-this.selectOptional.length>=0){
  let oCount=diff-this.selectOptional.length;
    this.feedB="You can select just "+oCount+" more Modules from Optional";
}
  }
  else{
    this.feedB="You can't select anymore";
  }
if(this.selectOptional.length==diff){
  this.showSelected=false;
  this.showOptional=false;
  this.feedB="";
  this.selectOptional.forEach(dat=>{

    this.optional.forEach(arr=>{
      let tempData=arr.code;

     if(dat.trim()==tempData.trim()){
       this.compulsory.push(arr);

     }
    });
  });
this.tempCompulsory=this.compulsory;
}
},
    removeModule(ski){
      this.selectOptional=this.selectOptional.filter(web=>web!==ski);
      if(this.selectOptional.length<1){
        this.showSelected=false;
      }
    },
changePrev(){

if(this.cCount>=1){
this.cCount=this.cCount-1;
this.data=this.compulsory[this.cCount-1];

}
if(this.cCount<=4){
  this.showModule=true;
  this.lastModule=false;
}


  if(this.cCount==0){
    if(this.transferHistorys.length>0 && this.startYear>(this.transferYear-3)){
      this.startYear=this.startYear-1;
      this.cCount=4;
      this.modulesShow();


    }
    if(this.transferHistorys.length<1){
      this.startYear=this.startYear-1;
      this.cCount=4;
      this.modulesShow();

    }
  }

  if(this.transferHistorys.length>0){
    if(this.cCount==1 && this.startYear==(this.transferYear-3)) {
      this.firstModule = true;
    }
  }
  if(this.cCount==1 && this.startYear==1){
    this.firstModule=true;
  }





},
    changeModule(){


      if(this.showSkills==true) {
        if (this.dataSkills == undefined || this.dataSkills == "") {
          this.feed = "Please enter the skills to continue";
        }
        else {

        let found = false;
        this.mdxSkill.forEach(dat => {
          if (dat.module == this.data.code) {
            dat.skill = this.dataSkills;
            dat.Grade = this.marks;
            found = true;
            this.feed = "Skill Updated";
          }
        })
        if (found == false) {
          this.mdxSkill.push({"module": this.data.code,"Grade":this.marks, "skill": this.dataSkills})
        }
        this.showSkills = false;
        this.marks = '';
        this.dataSkills = "";

        if (this.cCount <4) {
          this.data = this.compulsory[this.cCount];

        }
          if (this.cCount <5) {
            this.cCount = this.cCount + 1;
          }

        if(this.startYear==3 && this.cCount==5){
          this.lastModule=true;
          //this.data = this.optional[this.cCount-clength];

        }

        this.feed = "";

        if (this.cCount > 0) {
          this.firstModule = false;
        }
          if (this.cCount ==5) {
            if(this.startYear<3){
            this.startYear=this.startYear+1;
              this.cCount=1;
              this.modulesShow();

            }
            else {
           this.showDetails=false;
            }
          this.feedB="";

           // this.showModule=false;


          }


        }
      }
      else{
        this.feed="Please Generate Skills to go another module";
      }

    },
    skillsGenerate(){
if(this.marks!==undefined && this.marks!==""){
  try{
    this.showSkills=true;
    if(parseInt(this.marks)>=1 && parseInt(this.marks)<=20 ) {

      if (parseInt(this.marks) >= 1 && parseInt(this.marks) <= 4) {
        this.dataSkills = this.data.skill1;
      }
      if ((parseInt(this.marks)) >= 5 && (parseInt(this.marks)) <= 8) {
        this.dataSkills = this.data.skill2;
      }
      if (parseInt(this.marks) >= 9 && parseInt(this.marks) <= 12) {
        this.dataSkills = this.data.skill3;
      }
      if (parseInt(this.marks) >= 13 && parseInt(this.marks) <= 16) {
        this.dataSkills = this.data.skill4;
      }
      if (parseInt(this.marks) >= 17 && parseInt(this.marks) <= 20) {
        this.dataSkills = this.data.skill5;
      }
    }
    else{
      this.feed="Marks must be between 1-20";
      this.dataSkills="";
      this.showSkills=false;
    }
  }
  catch (e){
    this.feed="Marks must be between 1-20";
    this.dataSkills="";
    this.showSkills=false;


  }
}
else{
  this.feed="Please enter Marks";
}

    },

    modulesShow() {
      let id=2;
      if(this.mdxHistorys[0].course=="Computer Science"){
        id=1;
      }
      this.tempDetails=this.moduleDetail;
      this.tempDetails=this.tempDetails.filter(web=>web.title_id==id);

      if(this.startYear==1){
        this.miniTitle = 'First Year Modules';
        this.tempDetails=this.tempDetails.filter(web=>parseInt(web.year)==1);
        this.compulsory=this.tempDetails.filter(web=>web.optional=="N");
        this.optional=this.tempDetails.filter(web=>web.optional=="Y");
      }
      if(this.startYear==2){
        this.miniTitle = 'Second Year Modules';
        this.tempDetails=this.tempDetails.filter(web=>parseInt(web.year)==2);
        this.compulsory=this.tempDetails.filter(web=>web.optional=="N");
        this.optional=this.tempDetails.filter(web=>web.optional=="Y");
      }
      if(this.startYear==3){
        this.miniTitle = 'Third Year Modules';
        this.tempDetails=this.tempDetails.filter(web=>parseInt(web.year)==3);
        this.compulsory=this.tempDetails.filter(web=>web.optional=="N");
        this.optional=this.tempDetails.filter(web=>web.optional=="Y");
      }


      if(this.optional.length>0){
        this.showOptional=true;
        this.optionalData=[];
        this.optional.forEach(dat=>{
          this.optionalData.push(dat.code);
        });

        let diff=4-this.compulsory.length;
        if(this.selectOptional.length==diff){
          this.showOptional=false;
          this.selectOptional.forEach(dat=>{

            this.optional.forEach(arr=>{
              let tempData=arr.code;

              if(dat.trim()==tempData.trim()){
                this.compulsory.push(arr);

              }
            });
          });
        }
        else{

          this.selectOptional=[];
        }
      if(this.mdxSkill.length>0){


            this.optional.forEach(dat=>{
              this.mdxSkill.forEach(arr=>{
                if(dat.code.trim()==arr.module.trim()){
                  this.compulsory.push(dat);
                  this.showOptional=false;
                }

              });

            });





        }
      }



      this.data=this.compulsory[this.cCount-1];





    },


    selectLevel() {
  this.checkTransfer=false;
      this.showDetails = true;
      if (this.transferYear == 4) {
        this.miniTitle = 'First Year Modules';
        this.startYear = 1;
      }
      if (this.transferYear == 5) {
        this.miniTitle = 'Second Year Modules';
        this.startYear = 2;
      }
      if (this.transferYear == 6) {
        this.miniTitle = 'Third Year Modules';
        this.startYear = 3;
      }
      this.modulesShow();
    },


    prevPage: function () {

      this.$emit('changemdxSkill', this.mdxSkill);
      if (this.transferHistorys.length > 0) {
        this.$emit('changeTitle', 'Transfer Module Selection');
        this.$emit('changePage', 'TransferModules');
      } else {
        this.$emit('changeTitle', 'Skills');
        this.$emit('changePage', 'Skill');
      }


    },
    //lenght of mdxskiil while next and allow
    nextPage: function () {
      let req=3;
      if(this.transferHistorys.length>0){

        req=this.transferYear-3;
        req=4-req;
      }

      if((this.startYear==3 && this.cCount>4) || this.mdxSkill.length==(req*4) ){
        this.$emit('changemdxSkill', this.mdxSkill);

        this.$emit('changeTitle', 'Extras');
        if (this.transferHistorys.length > 0) {
          this.$emit('changeTransferYears', this.transferYear);
        }
        this.$emit('changePage', 'Extras');
        this.feed="";
      }
      else{
  this.feed="Please generate Skill for all Modules";
      }


    },


  },


}
</script>

