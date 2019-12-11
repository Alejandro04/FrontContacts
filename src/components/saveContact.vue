<template>
  <div>
    <p class="app_title">Save Contact</p>

    <div class="form-container">
      <div class="form-group">
        <div v-if="label_name_validate" class="validate">Please add the name</div>
        <input type="text" class="form-control" v-model="name" placeholder="Add the name" />
      </div>
      <div class="form-group">
        <div v-if="label_number_validate" class="validate">Please add the phone</div>
        <input type="number" class="form-control" v-model="number" placeholder="Add the phone" />
      </div>
      <div class="form-group">
        <button @click="saveContact" class="btn btn-success btn-lg">Save</button>
      </div>
      <div>
          <div v-if="msg_success" class="success">Contact save!
              See contact  <router-link to="/contacts">Contacts</router-link>
          </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      number: "",
      label_name_validate: false,
      label_number_validate: false,
      msg_success: false
    };
  },
  methods: {
    saveContact() {
      let validation = this.validation();
      if (validation == true) {
        axios
          .post("http://3.135.239.246/contacts", {
            name: this.name,
            number: this.number
          })
          .then((response) => {
            this.msg_success = true
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    validation() {
      if (this.name == "" || this.number == "") {
        if (this.name == "") {
          this.label_name_validate = true;
        } else {
          this.label_name_validate = false;
        }

        if (this.number == "") {
          this.label_number_validate = true;
        } else {
          this.label_number_validate = false;
        }

        return false;
      } else {
        return true;
      }
    }
  }
};
</script>

<style>
.app_title {
  font-size: 25px;
  margin-top: 50px;
}
.form-container {
  margin: auto;
}
.validate {
  color: red;
}
.success{
    font-size: 20px;
    color: green;
}

@media screen and (min-width: 480px) {
  .form-container {
    width: 90%;
  }
}

@media screen and (min-width: 767px) {
  .form-container {
    width: 70%;
  }
}

@media screen and (min-width: 950px) {
  .form-container {
    width: 50%;
  }
}
</style>

