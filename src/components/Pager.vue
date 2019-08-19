<template>
  <nav class="pagination is-centered" role="navigation" aria-label="pagination">
    <a
      :disabled="page === 1"
      @click="emitClickedPage(page - 1)"
      class="pagination-previous"
      >&lt;&lt;</a
    >
    <a
      :disabled="page === maxPage"
      @click="emitClickedPage(page + 1)"
      class="pagination-next"
      >&gt;&gt;</a
    >
    <ul class="pagination-list">
      <li v-for="(pageItem, index) in pageItems" :key="index">
        <template v-if="pageItem != null">
          <span
            v-if="pageItem === page"
            :aria-label="`Goto page ${pageItem}`"
            class="pagination-link is-current"
            aria-current="page"
            >{{ pageItem }}
          </span>
          <a
            v-else
            :aria-label="`Goto page ${pageItem}`"
            @click="emitClickedPage(pageItem)"
            class="pagination-link"
            >{{ pageItem }}</a
          >
        </template>
        <span v-else class="pagination-ellipsis">&hellip;</span>
      </li>
    </ul>
  </nav>
</template>

<script lang="ts">
import { Vue, Component, Emit, Prop } from "vue-property-decorator";

@Component({})
export default class Pager extends Vue {
  @Prop({ default: 1 })
  page!: number;

  @Prop({ default: 1 })
  maxPage!: number;

  get pageItems(): (number | null)[] {
    const tempPages: (number | null)[] = [this.page];
    if (this.page !== 1) {
      tempPages.unshift(this.page - 1);
    }
    if (this.page !== this.maxPage) {
      tempPages.push(this.page + 1);
    }

    if (tempPages[0] !== 1) {
      if (tempPages[0] !== 2) {
        tempPages.unshift(null);
      }
      tempPages.unshift(1);
    }
    if (tempPages[tempPages.length - 1] !== this.maxPage) {
      if (tempPages[tempPages.length - 1] !== this.maxPage - 1) {
        tempPages.push(null);
      }
      tempPages.push(this.maxPage);
    }
    return tempPages;
  }

  @Emit("pageClick")
  emitClickedPage(page: number) {
    return page;
  }
}
</script>
