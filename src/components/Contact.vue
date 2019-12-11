<template>
  <div>
    <AppNav></AppNav>
    <p class="app_title">Contacts</p>

    <loading :active.sync="isLoading" :is-full-page="fullPage"></loading>
    <div v-if="msg_delete" class="success">Contact out!</div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Phone</th>
          <th scope="col">Update</th>
          <th scope="col">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in contacts" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.number }}</td>
          <td>
            <button class="btn btn-success" @click="updateContact(item.id)">
              <i class="fa fa-edit"></i>
            </button>
          </td>
          <td>
            <button class="btn btn-danger" @click="deleteContact(item.id)">
              <i class="fa fa-trash"></i>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import axios from "axios";
// Import component
import Loading from "vue-loading-overlay";
// Import stylesheet
import "vue-loading-overlay/dist/vue-loading.css";

export default {
  mounted() {
    this.getContacts();
  },
  components: {
    Loading
  },
  data() {
    return {
      contacts: []
    };
  },
  methods: {
    async getContacts() {
      this.isLoading = true;

      try {
        const response = await axios.get("http://3.135.239.246/contacts");
        response.data.forEach(element => {
          this.contacts.push({
            id: element._id,
            name: element.name,
            number: element.number
          });
        });
        this.isLoading = false;
      } catch (error) {
        console.error(error);
      }
    },
    updateContact(id) {
      this.$router.push(`contacts/${id}`);
    },
    deleteContact(id) {
      axios
        .delete(`http://3.135.239.246/contacts/${id}`, {})
        .then(response => {
          this.contacts.splice(id, 1);
          this.msg_delete = true;
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style>
.success{
    font-size: 20px;
    color: green;
}

@media screen and (min-width: 480px) {
}

@media screen and (min-width: 767px) {
}

@media screen and (min-width: 950px) {
}
</style>

