<template>
  <v-dialog width="400px" v-model="modal">
    <v-btn class="warning" flat slot="activator">Редактировать</v-btn>

    <v-card>
      <v-container>
        <v-layout row>
          <v-flex xs12>
            <v-card-title>
              <h1 class="text--primary">Редактировать объявление</h1>
            </v-card-title>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>
        <v-layout row>
          <v-flex xs12>
            <v-card-text>
              <v-text-field 
                name="title" 
                label="Заголовок объявления" 
                type="text"
                v-model="editedTitle"
                required
                :rules="[v => !!v || 'Введите заголовок']"
              ></v-text-field>
              <v-textarea 
                name="description" 
                label="Описание объявления" 
                type="text"
                v-model="editedDescription"
                :rules="[v => !!v || 'Введите описание']"
              ></v-textarea>
            </v-card-text>
            <v-switch
              label="Добавить в топ?"
              v-model="promo"
              color="primary"
            >
            </v-switch>
          </v-flex>
        </v-layout>

        <v-layout row class="mb-3">
          <v-flex xs12>
            <v-btn class="warning" @click="triggerUpload">
              Загрузить изображение
              <v-icon right dark>cloud_upload</v-icon>
            </v-btn>
            <input 
              ref="fileInput" 
              type="file" 
              style="display: none;" 
              accept="image/*"
              @change="onFileChange"
            >
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs12>
            <img :src="imageSrc" height="100" v-if="imageSrc">
          </v-flex>
        </v-layout>

        <v-divider></v-divider>
        <v-layout row>
          <v-flex xs12>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn flat @click="onCancel">Отменить</v-btn>
              <v-btn class="success" flat @click="onSave">Сохранить</v-btn>
            </v-card-actions>
          </v-flex>
        </v-layout>

      </v-container>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['ad'],
  data () {
    return {
      modal: false,
      editedDescription: this.ad.description,
      editedTitle: this.ad.title,
      promo: this.ad.promo,
      imageSrc: this.ad.imageSrc,
      image: this.ad.image
    }
  },
  methods: {
    onCancel () {
      this.editedDescription = this.ad.description
      this.editedTitle = this.ad.title
      this.promo = this.ad.promo
      this.modal = false
    },
    onSave () {
      if (this.editedDescription !== '' && this.editedTitle !== '') {
        this.$store.dispatch('updateAd', {
          title: this.editedTitle,
          description: this.editedDescription,
          promo: this.promo,
          id: this.ad.id,
          image: this.image,
          imageSrc: this.imageSrc,
          imageExt: this.ad.imageExt
        })
        this.modal = false
      }
    },
    triggerUpload () {
      this.$refs.fileInput.click()
    },
    onFileChange (event) {
      const file = event.target.files[0]

      const reader = new FileReader()
      reader.onload = e => {
        this.imageSrc = reader.result
      }
      reader.readAsDataURL(file)
      this.image = file
    }
  }
}
</script>