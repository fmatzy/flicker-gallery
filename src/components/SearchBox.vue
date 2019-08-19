<template>
  <form @submit.prevent class="columns is-centered is-mobile">
    <div class="column is-10">
      <input
        v-model="inputText"
        class="input"
        type="text"
        placeholder="Flickrを検索"
      />
    </div>
    <div class="column is-narrow">
      <button @click="emitText()" class="button">検索</button>
    </div>
  </form>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from "vue-property-decorator";

@Component({})
export default class SearchBox extends Vue {
  @Prop()
  text!: string;

  @Watch("text")
  textChanged() {
    this.inputText = this.text;
  }

  inputText: string = this.text;

  @Emit("click")
  emitText() {
    return this.inputText;
  }
}
</script>
