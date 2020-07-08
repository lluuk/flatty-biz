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
              v-model="userEmail"
              :error-messages="emailErrors"
              label="Email"
              name="email"
              prepend-icon="mdi-account"
              type="email"
              required
              @input="v$.userEmail.$touch()"
              @blur="v$.userEmail.$touch()"
            ></v-text-field>

            <v-text-field
              id="password"
              v-model="password"
              :error-messages="passwordErrors"
              label="Password"
              name="password"
              prepend-icon="mdi-lock"
              type="password"
              required
              @change="v$.password.$touch()"
              @blur="v$.password.$touch()"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" :disabled="v$.$invalid" @click="handleSubmit"
            >Login</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { ref, computed } from '@vue/composition-api'
import useVuelidate from '@vuelidate/core'
import { required, minLength, email } from 'vuelidate/lib/validators'

export default {
  layout: 'guest',

  setup(props, { root }) {
    const userEmail = ref('')
    const password = ref('')

    const rules = {
      userEmail: { required, email, $autoDirty: true },
      password: { required, minLength: minLength(6), $autoDirty: true },
    }

    const v$ = useVuelidate(rules, {
      userEmail,
      password,
    })

    const emailErrors = computed(() => {
      const errors = []
      if (!v$.userEmail.$invalid || !v$.userEmail.$dirty) return errors
      v$.userEmail.email.$invalid && errors.push('Must be valid e-mail')
      v$.userEmail.required.$invalid && errors.push('E-mail is required')
      return errors
    })

    const passwordErrors = computed(() => {
      const errors = []
      if (!v$.password.$invalid || !v$.password.$dirty) return errors
      v$.password.minLength.$invalid &&
        errors.push('Password must be mininimum 6 characters long')
      v$.password.required.$invalid && errors.push('Password is required')
      return errors
    })

    const handleSubmit = async () => {
      const { $fireAuth, $router, $toast } = root
      try {
        await $fireAuth.signInWithEmailAndPassword(
          userEmail.value,
          password.value
        )
        $toast.success('Welcome back', { duration: 3000 })
        $router.go('/')
      } catch (e) {
        $toast.error(e, { duration: 3000 })
      }
    }

    return {
      userEmail,
      password,
      emailErrors,
      passwordErrors,
      handleSubmit,
      v$,
    }
  },
}
</script>
