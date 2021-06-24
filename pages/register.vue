<template>
  <v-container class="mt-2" style="min-height: 800px">
    <v-row align="center" justify="start">
      <v-col cols="4">
        <v-card
          class="mt-24"
          flat
          rounded
          color="background"
        >
          <v-card-title class="font-weight-bold text-h4">
            Créer un compte
          </v-card-title>
          <v-card-text>
            <v-form class="mt-12">
              <v-text-field
                v-model="registerPayload.firstName"
                :rules="requiredRules"
                rounded
                filled
                label="Prenom"
              />
              <v-text-field
                v-model="registerPayload.lastName"
                :rules="requiredRules"
                rounded
                filled
                label="Nom"
              />
              <v-text-field
                v-model="registerPayload.email"
                :rules="emailRules"
                rounded
                filled
                placeholder="exemple@mail.com"
                label="Adresse mail"
              />
              <v-text-field
                v-model="registerPayload.password"
                :rules="passwordRules"
                type="password"
                rounded
                filled
                label="Mot de passe"
              />
              <v-text-field
                v-model="registerPayload.referalCode"
                rounded
                filled
                label="Code de parrainage"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              rounded
              @click="login()"
            >
              Se connecter
            </v-btn>
            <v-btn
              text
              rounded
            >
              S'inscrire
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Register',
  data () {
    return {
      registerPayload: {
        referalCode: ''
      },

      // Règles de vérification
      requiredRules: [
        v => !!v || 'Ce champ est requis'
      ],
      emailRules: [
        v => !!v || 'Il faut rentrer un email',
        v => /.+@.+\..+/.test(v) || 'L\'email doit etre valide'
      ],
      phoneRules: [
        v => !!v || 'Il faut rentrer un numéro de télélphone',
        v => /^(0|\+33)[1-9]([-. ]?[0-9]{2}){4}$/.test(v) || 'Le numéro de téléphone doit être valide'
      ],
      passwordRules: [
        v => !!v || 'Il faut rentrer un numéro de télélphone',
        v => /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$ %^&*-]).{8,}$/.test(v) || 'Votre mot de passe doit contenir 8 Charactères dont une majuscule un chiffre et un charactère spécial'
      ]
    }
  },
  mounted () {
    this.registerPayload.referalCode = this.$route.query.referalCode
  },
  methods: {
    login () {
      this.$auth.loginWith('local', {
        data: {
          email: this.email,
          password: this.password
        }
      })
        .then((response) => {
          if (response.status !== 200) {
            this.$toast.error(response.data.message)
          }
        })
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.error(err)
          this.$toast.error(err.message)
        })
    }
  }
}
</script>

<style scoped>

</style>
