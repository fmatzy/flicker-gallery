<template>
  <div class="card">
    <div @click="toggleContent()" class="card-image">
      <figure class="image">
        <img :src="url" />
      </figure>
    </div>
    <div v-if="showContent" class="card-content" style="overflow: hidden">
      <p>
        <span class="title is-5">{{ title }}</span>
      </p>
      <nav class="level is-mobile">
        <div class="level-left">
          <a class="button">詳細</a>
        </div>
      </nav>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

export interface PhotoDatum {
  id: string;
  title: string;
  url_s: string;
}

@Component({})
export default class ImageItem extends Vue {
  @Prop({ required: true })
  title!: string;

  @Prop({ required: true })
  url!: string;

  showContent: boolean = false;

  toggleContent() {
    this.showContent = !this.showContent;
  }
}
</script>
