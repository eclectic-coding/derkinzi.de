<template lang="pug">
#home
  #blogroll
    ul.list-none
      li.mt-10.border-b.border-gray-400.pb-10(class="md:mt-20 md:pb-20" v-for="post in posts" :key="post.attributes.title")
        PostShort(:post="post")
</template>

<script>
import moment from 'moment'
import PostShort from '~/components/postshort'

export default {
  components: {
    PostShort
  },
  layout: 'intro',
  async asyncData() {
    const resolve = await require.context('~/content/posts/', true, /\.md$/)
    let imports = resolve.keys().map((key) => resolve(key))
    // filter out page type
    imports = imports.filter((post) => !post.attributes.type)
    // sort by date
    imports.sort((a, b) =>
      moment(b.attributes.date, 'DD/MM/YYYY').diff(
        moment(a.attributes.date, 'DD/MM/YYYY')
      )
    )
    return {
      posts: imports
    }
  }
}
</script>

<style></style>
