<template>
  <div>
    <div class="hero is-primary">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h3 class="title is-3 is-center">Flickr Gallery</h3>
          <SearchBox @click="onClick($event)" :text="keyword" />
          <SearchHistory
            :history="searchHistory"
            @historyClick="onHistoryClick($event)"
            @historyDelete="onHistoryDelete($event)"
          />
        </div>
      </div>
    </div>
    <div class="section">
      <div class="container">
        <ImageList :photoItems="photoItems" />
        <Pager
          :page="page"
          :maxPage="maxPage"
          @pageClick="onPagerClick($event)"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { Component, Vue } from "vue-property-decorator";
import ImageList, {
  PhotosResult,
  PhotoResult
} from "@/components/ImageList.vue";
import SearchBox from "@/components/SearchBox.vue";
import Pager from "@/components/Pager.vue";
import SearchHistory from "@/components/SearchHistory.vue";

const API_KEY = process.env.VUE_APP_FLICKR_KEY;
const REQUEST_URL = "https://api.flickr.com/services/rest";

@Component({
  components: {
    ImageList,
    SearchBox,
    Pager,
    SearchHistory
  }
})
export default class Home extends Vue {
  photoItems: PhotoResult[] = [];
  searchHistory: string[] = [];

  page: number = 1;
  maxPage: number = 1;

  keyword: string = "";

  onClick(text: string) {
    this._fetch({ keyword: text, page: 1 });
  }

  onPagerClick(page: number) {
    this._fetch({ page });
  }

  onHistoryClick(item: string) {
    this._fetch({ keyword: item, page: 1 });
  }

  onHistoryDelete(item: string) {
    this._removeFromHistory(item);
  }

  private _removeFromHistory(item: string) {
    const index = this.searchHistory.findIndex(value => value === item);
    if (index !== -1) {
      this.searchHistory.splice(index, 1);
    }
  }

  private _truncateHistory() {
    while (this.searchHistory.length > 10) {
      this.searchHistory.shift();
    }
  }

  private async _fetch(params: { keyword?: string; page?: number }) {
    const { keyword = this.keyword, page = this.page } = params;

    const {
      data: { photos }
    } = await axios.get<{ photos: PhotosResult; stat: string }>(REQUEST_URL, {
      params: {
        method: "flickr.photos.search",
        api_key: API_KEY,
        text: keyword,
        page: page,
        per_page: "12",
        format: "json",
        nojsoncallback: "1",
        extras: "url_s"
      }
    });

    console.log(photos);
    this.keyword = keyword;
    this._removeFromHistory(keyword);
    this.searchHistory.push(keyword);
    this._truncateHistory();
    this.page = photos.page || 1;
    this.maxPage = photos.pages || 1;
    this.photoItems.splice(0);
    this.photoItems.push(...photos.photo);
  }
}
</script>
