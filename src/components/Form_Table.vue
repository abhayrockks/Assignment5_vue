<template>
  <div id="table">
    <button id="show-modal" @click="showModal = true">SignUp</button>
    <Form
      v-bind:pname="name"
      v-bind:pemail="email"
      v-bind:pdob="dob"
      v-bind:ppassword="password"
      v-bind:editIndex="editIndex"
      v-on:editItem="edit"
      v-on:submitItem="submit"
      v-if="showModal"
      @close="showModal = false"
    />

    <table class="table">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Email</th>
          <th>
            DOB
            <select v-model="selected" @change="sortedItems(selected)">
              <option disabled value="default">Sort</option>
              <option>Ascending</option>
              <option>Descending</option>
            </select>
          </th>
          <th>Password</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in details" v-bind:key="index">
          <th>{{index}}</th>
          <td>{{item.name}}</td>
          <td>{{item.email}}</td>
          <td>{{item.dob}}</td>
          <td>{{item.password}}</td>
          <td>
            <a href="#" v-on:click.prevent="deleteItem(index)">Delete</a>
          </td>
          <td>
            <a href="#" v-on:click.prevent="editItem(index)">Edit</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import Form from "./Form.vue";
export default {
  name: "Form_Table",
  components: {
    Form
  },
  data() {
    return {
      showModal: false,
      name: "",
      email: "",
      dob: null,
      password: "",
      selected: "default",
      pname: "",
      pemail: "",
      pdob: "",
      ppassword: "",
      editIndex: -1,
      details: [
        {
          name: "Abhay",
          email: "abhay@gmail.com",
          dob: "2020-04-08",
          password: "mehta"
        },
        {
          name: "Rakesh",
          email: "rakesh@gmail.com",
          dob: "2020-12-23",
          password: "qwerty"
        }
      ]
    };
  },

  methods: {
    submit(value) {
      this.details.push({
        name: value.name,
        email: value.mail,
        dob: value.date,
        password: value.passwrd
      });
      (this.name = ""),
        (this.email = ""),
        (this.dob = null),
        (this.password = "");
    },

    edit(value) {
      this.details.splice(this.editIndex, 1, {
        name: value.name,
        email: value.mail,
        dob: value.date,
        password: value.passwrd
      });
      this.editIndex = -1;
      (this.name = ""),
        (this.email = ""),
        (this.dob = null),
        (this.password = "");
    },

    editItem(index) {
      this.showModal = true;
      this.editIndex = index;
      (this.name = this.details[index].name),
        (this.dob = this.details[index].dob),
        (this.email = this.details[index].email),
        (this.password = this.details[index].password);
    },

    deleteItem(index) {
      this.details.splice(index, 1);
    },

    sortedItems(order) {
      let key = "dob";
      if (order == "Ascending") {
        this.details.sort((a, b) => {
          var x = a[key];
          var y = b[key];
          return x < y ? -1 : x > y ? 1 : 0;
        });
      } else {
        this.details.sort((a, b) => {
          var x = a[key];
          var y = b[key];
          return x > y ? -1 : x < y ? 1 : 0;
        });
      }
    }
  }
};
</script>
<style>
.table {
  width: 50%;
  margin-top: 50px;
  margin-left: auto;
  margin-right: auto;
  border: solid;
  text-align: center;
}

.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
</style>