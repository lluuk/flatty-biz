<template>
  <v-row align="center" justify="center">
    <v-col cols="12" sm="8" md="4">
      <v-card class="elevation-12">
        <v-toolbar color="primary" dark flat>
          <v-toolbar-title>Login form</v-toolbar-title>
        </v-toolbar>
        <v-card-text>
          <v-form>
            <v-text-field
              v-model="email"
              label="Email"
              name="email"
              prepend-icon="mdi-account"
              type="email"
              required
              @input="$v.name.$touch()"
              @blur="$v.name.$touch()"
            ></v-text-field>

            <v-text-field
              id="password"
              v-model="password"
              label="Password"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              required
              @change="$v.checkbox.$touch()"
              @blur="$v.checkbox.$touch()"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="handleSubmit">Login</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { ref } from '@vue/composition-api'
export default {
  layout: 'guest',
  setup() {
    const email = ref('')
    const password = ref('')
    const emailErrors = ref(false)
    const passwordErrors = ref(false)

    // const setEmail = (email) => {
    //   email.value = email
    // }

    // const setPassword = (password) => {
    //   password.value = password
    // }

    const handleSubmit = () => {
      console.log({ email, password })
    }

    return {
      email,
      password,
      emailErrors,
      passwordErrors,
      handleSubmit,
      // setEmail,
      // setPassword,
    }
  },
}
</script>
