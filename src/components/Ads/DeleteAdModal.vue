<template>
  <div class="text-xs-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <v-btn
        slot="activator"
        class="error"
        flat
      >
        Удалить
      </v-btn>

      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          Удалить объявление?
        </v-card-title>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            Отменить
          </v-btn>
          <v-btn
            color="error"
            flat
            @click="onDelete"
          >
            Удалить
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
  export default {
    props: ['ad'],
    data () {
      return {
        dialog: false
      }
    },
    methods: {
      onDelete () {
        this.dialog = false
        this.$store.dispatch('deleteAd', {
          id: this.ad.id,
          imageExt: this.ad.imageExt
        })
        .then(() => {
          this.$router.push('/list')
        })
        .catch(() => {})
      }
    }
  }
</script>