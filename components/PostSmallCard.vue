<template>
  <li class="bg-white px-4 shadow py-4 rounded-md sm:rounded-lg">
    <article aria-labelledby="question-title-81614">
      <div class="flex justify-between">
        <h2 id="question-title-81614" class="text-base font-medium text-gray-900">
          {{ truncateString(post.title, 20) }}
        </h2>
        <span class="text-xs text-gray-600">{{ posted }}</span>
      </div>
      <div class="mt-1 show-white-space text-sm text-gray-700 space-y-4">{{ truncateString(post.description.trim(), 20) }}</div>
      <div class="mt-1 flex justify-between space-x-2">
        <div class="flex space-x-2 overflow-auto">
          <span v-if="post.city" class="inline-flex items-center px-2.5 py-0.5 rounded-md text-sm font-medium bg-green-100 text-green-800">
            {{ post.city }}
          </span>
          <span v-for="tag in tags" :key="tag.identifier" class="inline-flex items-center px-2.5 py-0.5 rounded-md text-sm font-medium bg-purple-100 text-purple-800">
            {{ $t('tags.' + tag.identifier) }}
          </span>
        </div>
        <div class="flex text-sm">
          <span class="inline-flex items-center text-sm">
          </span>
        </div>
      </div>
    </article>
  </li>
</template>

<script>
import formatDistance from 'date-fns/formatDistance'
import allTags from '@/assets/tags'

export default {
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  computed: {
    posted() {
      return formatDistance(new Date(Math.floor(this.post.created/1e6)), new Date(), {addSuffix: true})
    },
    tags() {
      const customTags = []
      this.post.tag.split('_').forEach((tag) => {
        if (tag.length > 0) {
          const xtag = allTags.filter((xtag) => xtag.identifier === tag.trim())
          if (xtag.length > 0)
            customTags.push(xtag[0])
        }
      })
      return customTags
    }
  },
  created() {
  },
  methods: {
    truncateString(str, num) {
      if (str.length <= num) {
        return str
      }
      return str.slice(0, num) + '...'
    }

  }
}
</script>

<style scoped>
.show-white-space {
    white-space: pre-wrap;
}
</style>
