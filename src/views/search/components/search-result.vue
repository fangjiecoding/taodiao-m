<template>
  <div class="search-result">
    <van-list v-model="loading"
              :finished="finished"
              finished-text="没有更多了"
              @load="onLoad"
              :error.sync="error"
              error-text="请求失败，点击重新加载">
      <van-cell v-for="(article,index) in list"
                :to="`/article/${article.art_id}`"
                :key="index"
                :title="article.title" />
    </van-list>
  </div>
</template>

<script>
import { getSearchResult } from "@/api/search"
export default {
  name: 'SearchResult',
  props: {
    SearchText: {
      type: String,
      required: true
    },
  },
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
      page: 1,
      per_page: 20,
      error: false
    };
  },
  methods: {
    async onLoad () {
      try {
        const { data } = await getSearchResult({
          page: this.data,
          per_page: this.per_page,
          q: this.SearchText
        })
        const { results } = data.data
        this.list.push(...results)
        this.loading = false
        if (results.length) {
          this.page++
        } else {
          this.finished = true
        }
      } catch (err) {
        this.error = true
        this.loading = false
      }
    },
  },
}
</script>

<style>
</style>