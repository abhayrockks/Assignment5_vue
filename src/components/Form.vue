<template>
  <div id="submitForm">
    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-container">
            <div class="modal-body">
              <form v-on:submit.prevent="submit">
                <div class="form-group">
                  <label>Name:</label>
                  <input type="text" v-model="name" required />
                </div>
                <br />

                <div class="form-group">
                  <label>Email:</label>
                  <input type="email" v-model="email" required />
                </div>
                <br />

                <div class="form-group">
                  <label>Dob:</label>
                  <input type="date" v-model="dob" required />
                </div>
                <br />

                <div class="form-group">
                  <label>Password:</label>
                  <input type="password" v-model="password" required />
                </div>
                <br />

                <div class="form-group">
                  <label>Confirm Password:</label>
                  <input type="password" v-model="confirm_password" required />
                </div>
                <br />

                <div class="form-group">
                  <label>Agree to terms and conditions</label>
                  <input type="checkbox" v-model="checked" required />
                </div>
                <br />
                <button>Add</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Form",
  data: function() {
    return {
      name: this.pname,
      email: this.pemail,
      dob: this.pdob,
      password: this.ppassword,
      confirm_password: "",
      checked: false
    };
  },

  props: {
    pname: { type: String, default: "" },
    pemail: { type: String, default: "" },
    pdob: { type: String, default: "" },
    ppassword: { type: String, default: "" },
    editIndex: { type: Number }
  },
  methods: {
    submit() {
      if (this.password !== this.confirm_password) {
        return alert("Both Password should match");
      }
      const payload = {
        name: this.name,
        mail: this.email,
        date: this.dob,
        passwrd: this.password
      };
      if (this.editIndex !== -1) {
        this.$emit("editItem", payload);
      } else {
        this.$emit("submitItem", payload);
      }
      this.$emit("close");
    }
  }
};
</script>

<style>
</style>