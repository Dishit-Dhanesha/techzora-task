<template>
  <div class="modal-card" style="width: auto">
    <header class="modal-card-head">
      <p class="modal-card-title">Edit {{ faq.question }}</p>
    </header>
    <section class="modal-card-body">
      <b-field label="Title">
        <b-input
          type="text"
          v-model="question"
          placeholder="Edit Question here"
        >
        </b-input>
      </b-field>

      <b-field label="Priority">
        <b-input type="text" v-model="answer" placeholder="Edit Answer here">
        </b-input>
      </b-field>
    </section>
    <footer class="modal-card-foot">
      <button class="button" type="button" @click="$parent.close()">
        Close
      </button>
      <button class="button is-primary" @click="editFaq">Save</button>
    </footer>
  </div>
</template>

<script>
export default {
  name: "EditModalView",
  props: {
    faq: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      question: "",
      answer: "",
    };
  },
  mounted() {
    this.question = this.faq.question;
    this.answer = this.faq.answer;
  },
  methods: {
    editFaq() {
      if (this.question && this.answer) {
        const payload = {
          id: this.faq.id,
          question: this.question,
          answer: this.answer,
        };
        this.$emit("edit-faq", payload);
      } else {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Something's not good, Please provide both Question and Answer`,
          position: "is-bottom",
          type: "is-danger",
        });
      }
    },
  },
};
</script>

<style scoped></style>
