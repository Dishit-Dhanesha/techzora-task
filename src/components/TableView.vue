<template>
  <div>
    <section class="section">
      <div class="container">
        <div class="level">
          <b-button class="is-primary" @click="isAddModalActive = true">
            Add a FAQ
          </b-button>
        </div>
        <div>
          <b-field class="container is-half column" label="Search">
            <b-input v-model="search"></b-input>
          </b-field>
        </div>
        <b-table :bordered="true" :data="filteredItems" default-sort="priority">
          <b-table-column v-slot="props" label="ID" width="40" sortable numeric>
            {{ props.row.id }}
          </b-table-column>

          <b-table-column v-slot="props" label="Question" sortable>
            {{ props.row.question }}
          </b-table-column>

          <b-table-column v-slot="props" label="Answer" sortable>
            {{ props.row.answer }}
          </b-table-column>

          <b-table-column v-slot="props" label="Edit">
            <b-button
              type="is-text"
              icon-left="pencil"
              @click="openEditModal(props.row)"
            ></b-button>
          </b-table-column>

          <b-table-column v-slot="props" label="Delete">
            <b-button
              type="is-text"
              icon-left="delete"
              @click="deleteFaq(props.row)"
            ></b-button>
          </b-table-column>
        </b-table>
      </div>
    </section>

    <b-modal :active.sync="isEditModalActive" has-modal-card>
      <edit-modal-view
        :faq="selectedFaq"
        @edit-faq="onEditFaq"
      ></edit-modal-view>
    </b-modal>

    <b-modal :active.sync="isAddModalActive" has-modal-card>
      <add-modal-view @add-faq="onAddFaq"></add-modal-view>
    </b-modal>
  </div>
</template>

<script>
import EditModalView from "@/components/EditModalView";
import AddModalView from "@/components/AddModalView";

export default {
  name: "TableView",
  components: { EditModalView, AddModalView },
  data() {
    return {
      initialFaq: [
        {
          id: 1,
          question: "What is the Colour of Sky ?",
          answer: "Blue",
        },
        {
          id: 2,
          question: "Name a Good Movie of Tom Cruise ?",
          answer: "Top Gun",
        },
      ],
      faqs: [],
      isEditModalActive: false,
      selectedFaq: {},
      isAddModalActive: false,
      search: "",
    };
  },
  mounted() {
    if (localStorage.getItem("faqs")) {
      this.faqs = JSON.parse(localStorage.getItem("faqs"));
    } else {
      this.faqs = this.initialFaq;
    }
  },
  computed: {
    filteredItems() {
      return this.faqs.filter((item) => {
        return (
          item.question.toLowerCase().indexOf(this.search.toLowerCase()) > -1 ||
          item.answer.toLowerCase().indexOf(this.search.toLowerCase()) > -1
        );
      });
    },
  },
  methods: {
    openEditModal(faq) {
      this.selectedFaq = faq;
      this.isEditModalActive = true;
    },
    onAddFaq(item) {
      // get the highest number id to iterate on it
      const highestId = Math.max.apply(
        Math,
        this.faqs.map((item) => item.id)
      );
      // Add the item to the array
      this.faqs.push({
        id: highestId + 1,
        question: item.question,
        answer: item.answer,
      });
      // save the updated array in localstorage
      this.saveLocalStorageFaq();
      this.isAddModalActive = false;
    },
    onEditFaq(item) {
      const faq = this.findFaq(item);
      // Apply the updated values
      faq.question = item.question;
      faq.answer = item.answer;
      // save the updated array in localstorage
      this.saveLocalStorageFaq();
      // close the modal
      this.isEditModalActive = false;
    },
    deleteFaq(item) {
      this.$buefy.dialog.confirm({
        title: `Deleting FAQ`,
        confirmText: "Delete FAQ",
        type: "is-danger",
        hasIcon: true,
        message: `Are you sure you want to delete this item? This cannot be undone.`,
        onConfirm: () => {
          // find in the array and remove
          const index = this.faqs.indexOf(item);
          this.faqs.splice(index, 1);
          // save the updated array in localstorage
          this.saveLocalStorageFaq();
        },
      });
    },
    findFaq(item) {
      return this.faqs.find((faq) => faq.id === item.id);
    },
    saveLocalStorageFaq() {
      localStorage.setItem("faqs", JSON.stringify(this.faqs));
      this.faqs = JSON.parse(localStorage.getItem("faqs"));
    },
  },
};
</script>

<style lang="scss" scoped></style>
