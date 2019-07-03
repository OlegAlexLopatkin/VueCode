<template>
	<v-container>
		<v-layout row>
			<v-flex xs12 v-if="!loading">
        <v-card v-if="ad">
          <v-card-media
            :src="ad.imageSrc"
            contain
            height="300px"
          ></v-card-media>
          <v-card-text>
            <h1 class="text--primary">{{ad.title}}</h1>
            <p>{{ad.description}}</p>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <addEditModal :ad="ad" v-if="isOwner"></addEditModal>
            <deleteAdModal :ad="ad" v-if="isOwner" class="ml-2">Удалить</deleteAdModal>
            <app-buy-modal :ad="ad" v-if="!isOwner"></app-buy-modal>
          </v-card-actions>
        </v-card>
        <v-card v-else>
          <v-card-text>
            <h1 class="text--primary text-xs-center">Этого объявления не существует</h1>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn flat :to="'/'">На главную</v-btn>
          </v-card-actions>
        </v-card>
        
			</v-flex>
      <v-flex xs12 v-else class="text-xs-center">
        <v-progress-circular
          indeterminate
          :size="100"
          :width="4"
          color="purple"
        ></v-progress-circular>
      </v-flex>
		</v-layout>
	</v-container>
</template>

<script>
import EditAdModal from './EditAdModal'
import DeleteAdModal from './DeleteAdModal.vue'
export default {
  props: ['id'],
  computed: {
    ad () {
      const id = this.id
      return this.$store.getters.adById(id)

    },
    loading () {
      return this.$store.getters.loading
    },
    isOwner () {
      if (this.$store.getters.user) {
        return this.ad.ownerId === this.$store.getters.user.id
      }
      return false
    }
  },
  components: {
    addEditModal: EditAdModal,
    deleteAdModal: DeleteAdModal
  }
}
</script>