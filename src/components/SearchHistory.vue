<template>
  <div class="level">
    <div class="level-left">
      <div v-for="item in historyList" :key="item" class="level-item">
        <div class="tags has-addons">
          <a @click="emitHistoryClick(item)" class="tag is-medium">{{
            item
          }}</a>
          <a
            @click="emitHistoryDelete(item)"
            class="tag is-medium is-delete"
          ></a>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit } from "vue-property-decorator";

@Component({})
export default class SearchHistory extends Vue {
  @Prop()
  history!: string[];

  get historyList() {
    return [...new Set(this.history)];
  }

  @Emit("historyClick")
  emitHistoryClick(item: string) {
    return item;
  }

  @Emit("historyDelete")
  emitHistoryDelete(item: string) {
    return item;
  }
}
</script>
