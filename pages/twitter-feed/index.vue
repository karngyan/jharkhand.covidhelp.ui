<template>
<div class="bg-gray-50">
  <div class="mx-auto py-28 grid grid-cols-1 gap-6 px-4 sm:px-6 lg:px-8 max-w-7xl lg:grid-flow-col-dense lg:grid-cols-3">
    <section aria-labelledby="right section" class="lg:col-start-1 lg:col-span-1">
      <b-field :label="$t('state')">
        <b-select placeholder="Select one" expanded v-model="state">
          <option v-for="state in states" :value="state" :key="state">{{ state }}</option>
        </b-select>
      </b-field>
      <div @click="state = ''" class="cursor-pointer font-sm pb-4 text-purple-600 hover:text-purple-800">{{ $t('clearState') }}</div>

      <b-field :label="$t('city')">
        <b-select placeholder="Select one" expanded v-model="city">
          <option v-for="city in cities" :value="city" :key="city">{{ city }}</option>
        </b-select>
      </b-field>

      <div @click="city = ''" class="cursor-pointer font-sm pb-4 text-purple-600 hover:text-purple-800">{{ $t('clearCity') }}</div>

      <b-field :label="$t('filter')" class="overflow-auto">
        <div class="flex flex-col">
          <span v-for="tag in tags" :key="tag.identifier">
            <b-switch v-model="tag.status" >{{ $t('tags.' + tag.identifier) }}</b-switch>
          </span>
        </div>
      </b-field>
    </section>
    <div class="space-y-6 lg:col-start-2 lg:col-span-3">
      <section aria-labelledby="posts">
        <ul class="posts space-y-4">
          <TwitterPost v-for="post in posts" :key="post.id" :post="post" />
        </ul>
      </section>
      <div class="flex items-center justify-center">
        <button @click="loadMoreClicked" v-if="postData.maxPage !== page && postData.maxPage > 1" type="button" class="inline-flex items-center px-4 py-2 border border-gray-300 shadow-sm text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          Load More
        </button>
      </div>

      </div>

  </div>
</div>
</template>

<script>
import tags from '@/assets/tags'
import stateCity from "@/assets/state-city"

export default {
  layout: 'default',
  computed: {
    tagString() {
      let res = ''
      this.tags.forEach((tag) => {
        if (tag.status)
          res += tag.identifier + "_"
      })
      this.tag = res
      return res
    },
    cities() {
      return stateCity[this.state]
    }
  },
  data() {
    return {
      tags: tags,
      posts: [],
      postData: {
        page: 1,
        posts: [],
        minPage: 1,
        maxPage: 1
      },
      tag: '',
      page: 1,
      city: '',
      state: '',
      states: Object.keys(stateCity),
    }
  },
  methods: {
    fetchPosts() {
      const store = this.$store
      this.tag = this.tagString
      console.debug('fetching posts')
      store.dispatch('fetchTwitterPosts', {tag: this.tag, page: this.page, city: this.city, state: this.state})
        .then((data) => {
          this.postData = data
          this.posts.push(...data.posts)
          // store.dispatch('success', 'Posts fetch success.')
        }).catch((error) => {
          store.dispatch('danger', 'Some error occurred while fetching posts.')
      })
    },
    loadMoreClicked() {
      this.page += 1
      this.fetchPosts()
    }
  },
  created() {
    this.fetchPosts()
  },
  watch: {
    tags: {
      handler: function() {
        this.posts = []
        this.page = 1
        this.fetchPosts()
      },
      deep: true
    },
    city() {
      this.posts = []
      this.page = 1
      this.fetchPosts()
    },
    state() {
      this.posts = []
      this.page = 1
      this.fetchPosts()
    }
  }
}
</script>

<style scoped>

</style>
