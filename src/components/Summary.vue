<template>
  <div>
    <div class="wrapper">
      <div>
        <h2 id="requireText">Summary</h2> <a id="summaryCount">{{this.summaryCount}}/100</a>
      </div>
      <textarea v-model="summaryText" spellcheck="true" placeholder="Summary about yourself......" required  v-on:input="summaryChange" value=summaryText></textarea>
    </div>
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
  name: "Summary",
  props:["summary"],
  data() {
    return {
      summaryText:'',
      buttonText:'Next',
      summaryCount: 0,
      feed:'',


    }
  },
  created() {
    if (this.summary.length !== undefined) {
      this.summaryText=this.summary;
      this.summaryCount=this.summaryText.length;

    }

  },
  methods: {
    summaryChange: function () {
      if (this.summaryText.length !== undefined) {
        this.summaryCount = this.summaryText.length;
      }
      const textarea = document.querySelector("textarea");
      textarea.addEventListener("keyup", e => {
        textarea.style.height = "300px";
        let scHeight = e.target.scrollHeight;
        textarea.style.height = `${scHeight}px`;
      });
    },
    nextPage: function () {
      if (this.summaryText !== undefined && this.summaryText.length >= 100) {
        this.feed = "";
        this.$emit('changeTitle','Education and Websites link');
        this.$emit('changeSummary',this.summaryText);
        this.$emit('changePage', 'Education');

      } else {
        this.feed = "Please fill atleast 100 characters";
      }


    },
    prevPage: function () {
      this.$emit('changeTitle','Personal Details');
      this.$emit('changePage', 'PersonalDetails')

    },
  }
}
</script>

