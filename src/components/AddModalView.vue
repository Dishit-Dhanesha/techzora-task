<template>
  <div class="modal-card" style="width: auto">
    <header class="modal-card-head">
      <p class="modal-card-title">Add FAQ</p>
    </header>
    <section class="modal-card-body">
      <b-field label="Question">
        <b-input
          type="text"
          v-model="question"
          placeholder="Your Question here"
        >
        </b-input>
      </b-field>

      <b-field label="Answer">
        <b-input type="text" v-model="answer" placeholder="Your Answer here">
        </b-input>
      </b-field>
    </section>
    <footer class="modal-card-foot">
      <button class="button" type="button" @click="$parent.close()">
        Close
      </button>
      <button class="button is-primary" @click="addFAQ">Save</button>
    </footer>
  </div>
</template>

<script>
export default {
  name: "ModalView",
  data() {
    return {
      answer: "",
      question: "",
    };
  },
  methods: {
    addFAQ() {
      if (this.question && this.answer) {
        const payload = {
          question: this.question,
          answer: this.answer,
        };
        this.$emit("add-faq", payload);
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
