<template>
  <div>
    <button id="show-modal" @click="signup">SignUp</button>
    <Form
      v-bind:pname="name"
      v-bind:pemail="email"
      v-bind:pdob="dob"
      v-bind:ppassword="password"
      v-bind:editIndex="selectedIndex"
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
          <th colspan="2">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in displayedPosts" v-bind:key="index">
          <th>{{item.id}}</th>
          <td>{{item.name}}</td>
          <td>{{item.email}}</td>
          <td>{{item.dob}}</td>
          <td>{{item.password}}</td>
          <td>
            <a href="#" v-on:click.prevent="deleteItem(item.id)">Delete</a>
          </td>
          <td>
            <a href="#" v-on:click.prevent="editItem(item.id)">Edit</a>
          </td>
        </tr>
      </tbody>
    </table>
    <p type="button" class="page-link">{{page}}</p>
    <button type="button" class="page-link" v-if="page != 1" @click="page--">Previous</button>
    <button type="button" class="page-link" v-if="page < numberOfPages" @click="page++">Next</button>
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
      id: "",
      name: "",
      email: "",
      dob: null,
      password: "",
      selected: "default",
      selectedIndex: -1,
      editedId: 0,
      details: [
        {
          id: 0,
          name: "Abhay",
          email: "abhay@gmail.com",
          dob: "1995-04-08",
          password: "mehta"
        },
        {
          id: 1,
          name: "Rakesh",
          email: "rakesh@gmail.com",
          dob: "1997-12-23",
          password: "qwerty"
        },
        {
          id: 2,
          name: "Ashish",
          email: "ashish@gmail.com",
          dob: "1995-10-09",
          password: "ashish"
        },
        {
          id: 3,
          name: "Ritika",
          email: "ritika@gmail.com",
          dob: "1994-01-13",
          password: "ritika"
        },
        {
          id: 4,
          name: "Evan",
          email: "evan@gmail.com",
          dob: "1995-05-16",
          password: "evan"
        }
      ],
      newId: 5,
      page: 1,
      perPage: 5,
      numberOfPages: 0,
      pages: []
    };
  },

  methods: {
    signup() {
      (this.editedId = 0), (this.selectedIndex = -1);
      (this.name = ""),
        (this.email = ""),
        (this.dob = null),
        (this.password = "");
      this.showModal = true;
    },

    searchindex(Id) {
      for (var i = 0; i < this.details.length; i++) {
        if (this.details[i].id === Id) {
          this.selectedIndex = i;
          break;
        }
      }
    },

    submit(value) {
      this.details.push({
        id: this.newId++,
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
      this.details.splice(this.selectedIndex, 1, {
        id: this.editedId,
        name: value.name,
        email: value.mail,
        dob: value.date,
        password: value.passwrd
      });
      (this.editedId = 0),
        (this.name = ""),
        (this.email = ""),
        (this.dob = null),
        (this.password = "");
    },

    editItem(Id) {
      this.searchindex(Id);
      (this.editedId = this.details[this.selectedIndex].id),
        (this.name = this.details[this.selectedIndex].name),
        (this.dob = this.details[this.selectedIndex].dob),
        (this.email = this.details[this.selectedIndex].email),
        (this.password = this.details[this.selectedIndex].password);
      this.showModal = true;
    },

    deleteItem(Id) {
      this.searchindex(Id);
      this.details.splice(this.selectedIndex, 1);
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
    },

    setPages() {
      this.numberOfPages = this.details.length / this.perPage;
      for (let index = 1; index <= this.numberOfPages; index++) {
        this.pages.push(index);
      }
    },

    paginate(data) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return data.slice(from, to);
    }
  },

  computed: {
    displayedPosts() {
      return this.paginate(this.details);
    }
  },

  watch: {
    details() {
      this.setPages();
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
  text-align: center;
}

table,
th,
td {
  border: solid 1px #ddd;
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

button.page-link {
  display: inline-block;
}

p,
button.page-link {
  font-size: 20px;
  color: #29b3ed;
  font-weight: 500;
}
</style>