<template>
  <div class="columns is-multiline">
    <template v-if="photoItems && photoItems.length > 0">
      <div
        v-for="photo in photoItems"
        :key="photo.id"
        class="column is-4-desktop is-6-tablet"
      >
        <ImageItem :title="photo.title" :url="photo.url_s" />
      </div>
    </template>
    <div v-else class="column is-full has-text-centered">
      <span class="title is-4 has-text-grey">No items</span>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import ImageItem from "./ImageItem.vue";

export interface PhotoResult {
  id: string;
  title: string;
  url_s: string;
}

export interface PhotosResult {
  page: number;
  pages: number;
  perpage: number;
  total: number;
  photo: PhotoResult[];
}

@Component({
  components: {
    ImageItem
  }
})
export default class ImageList extends Vue {
  @Prop()
  photoItems!: PhotoResult[];
}
</script>
