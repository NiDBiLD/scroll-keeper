<template>
  <div class="faux-modal-container">
    <div class="card dark padding-small faux-modal">
      <h2>Create an Account</h2>
      <form>
        <p>
          <input
            type="text"
            placeholder="Email"
            class="margin-bottom-small"
            v-model="email"
          />
        </p>
        <p>
          <input
            type="text"
            placeholder="Name"
            class="margin-bottom-small"
            v-model="username"
          />
        </p>
        <p>
          <input
            type="password"
            placeholder="Password"
            class="margin-bottom-small"
            v-model="password"
          />
        </p>
      </form>
      <p class="align-right"><button @click="register">Submit</button></p>
    </div>
  </div>
</template>

<script>
import { ref as vRef } from "vue";
import { ref as fbRef, getDatabase, set } from "firebase/database";
import { getAuth, createUserWithEmailAndPassword, updateProfile } from "firebase/auth";
import { useRouter } from "vue-router";

export default {
  setup() {
    const db = getDatabase();
    const email = vRef("");
    const username = vRef("");
    const password = vRef("");
    const router = useRouter();
    return {
      db,
      email,
      username,
      password,
      router,
    };
  },
  methods: {
    register() {
      const auth = getAuth();
      createUserWithEmailAndPassword(auth, this.email, this.password)
        .then((userCredential) => {
          const user = userCredential.user;
          updateProfile(user, {
            displayName: this.username
          });

          // user field "moderator" is a hack, should be handled via manipulation of auth/user obj)
          this.writeUserData(user.uid, this.username, this.email);

          this.router.push("/");
        })
        .catch((error) => {
          console.log(error.code);
          alert(error.message);
        });
    },
    writeUserData(userUid, username, email) {
      // Database contact (WILL OVERWRITE CURRENT)
      set(fbRef(this.db, "users/" + userUid), {
        username: username,
        email: email,
      });
    },
  },
};
</script>
