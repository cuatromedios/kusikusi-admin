<template>
  <div>
    <div v-if="loading" class="row q-col-gutter-md">
      <div class="col-xs-12 col-sm-6 col-md-4" v-for="index in 3" :key="index">
        <q-card flat bordered class="full-width">
          <q-skeleton height="200px" square animation="fade" />
          <q-card-section>
            <q-skeleton type="text" class="text-subtitle2" animation="fade" />
            <q-skeleton type="text" width="50%" class="text-subtitle2" animation="fade" />
          </q-card-section>
        </q-card>
      </div>
    </div>
    <div v-if="!loading">
      <div class="q-mb-md flex justify-end">
        <q-btn-dropdown class="" outline color="positive"  icon="filter_alt"  :label="'Filtrar'">
          <q-list>
            <q-item clickable v-close-popup
                    v-for="filter in filters"
                    :key="filter">
              <q-item-section>
                <q-item-label>{{ filter.label }}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-btn-dropdown>
      </div>
      <draggable class="row draggable-container q-col-gutter-md">
        <div v-for="medium in media" :key="medium.id"
        class="col-xs-12 col-sm-6 col-md-4">
          <medium-item :medium="medium"
                      :entity_id="[]"
                      :tags="[]"
                      :allowed="[]"
                      :reorderMode="[]"
                      @getMedia="getMedia"
                      class="full-width full-height" />
        </div>
      </draggable>  
    </div>
  </div>
</template>
<script>
import _ from 'lodash'
import MediumItem from './MediumItem'
export default {
  components: { MediumItem },
  name: 'MediaLibrary',
  data () {
    return {
      loading: true,
      media: [],
      filters:{
        one:{
          label: 'All'
        },
        two:{
          label: 'Two'
        },
        three:{
          label: 'Three'
        },
        four:{
          label: 'Four'
        }
      }
    }
  },
  mounted () {
    this.getMedia()
  },
  methods: {
    async getMedia () {
      this.loading = true
      const mediaResult = await this.$api.get(`/entities/medium?select=contents.title,properties,is_active,model,id&only-published=false&per-page=100`)
      console.log(mediaResult)
      this.loading = false
      if (mediaResult.success) {
        this.media = mediaResult.data.data
      } else {
        this.$q.notify({
          position: 'top',
          color: 'negative',
          message: this.$t('general.serverError')
        })
      }
    },
    addFilter(filter){
    }
  },
  computed: {
  
  }
}
</script>

<style lang="scss">
  
</style>
