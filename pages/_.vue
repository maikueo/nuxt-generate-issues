<template>
  <div>
    <h2>Items</h2>
    <div v-for="(item, index) in items" :key="index">
      {{ item.title }}
    </div>

    <h3>Pagination:</h3>
    <ul class="pagination">
      <li v-for="(pageNumber, index) in totalPages" :key="index">
        <nuxt-link :to="{ query: { page: pageNumber } }">{{
          pageNumber
        }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
const data = [
  {
    title: 'item 1',
  },
  {
    title: 'item 2',
  },
  {
    title: 'item 3',
  },
  {
    title: 'item 4',
  },
]

const fetchData = async (page) => {
  await Promise.resolve()
  return [data[page - 1]]
}

export default {
  async fetch() {
    const items = await fetchData(this.currentPageInUrl)
    this.items = items
    this.currentPage = this.currentPageInUrl
  },
  data() {
    return {
      items: [],
      totalPages: 4,
      currentPage: 1,
    }
  },
  computed: {
    currentPageInUrl() {
      return +this.$route.query.page || 1
    },
  },
  watch: {
    '$route.query': '$fetch',
  },
}
</script>
