<template>
<draggable :list="tickets" @change="handleChangeCard" :group="{ name: 'cards' }" class="min-h-full space-y-2">
  <div class="card bg-white overflow-auto h-24 rounded-md flex flex-col justify-between" v-for="card in cards" :key="card.id">
    <p class="font-medium text-base text-gray-600 px-2 py-1 line-clamp-2">
      {{ card.card_name }}
    </p>

    <!-- <div class="flex flex-none justify-end px-3 py-2">
      <p class="mr-2 text-gray-700">LT-8</p>
      <svg xmlns="http://www.w3.org/2000/svg" class="h-7 w-7 flex-none text-pink-400" viewBox="0 0 20 20" fill="currentColor">
        <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-6-3a2 2 0 11-4 0 2 2 0 014 0zm-2 4a5 5 0 00-4.546 2.916A5.986 5.986 0 0010 16a5.986 5.986 0 004.546-2.084A5 5 0 0010 11z" clip-rule="evenodd" />
      </svg>
    </div> -->
  </div>
</draggable>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'Card',
  props: {
    cards: Array,
    pipelineId: {
      type: Number,
    }
  },
  components: {
    draggable,
  },
  data() {
    return {
      tickets: [],
    }
  },
  mounted() {
    this.tickets = this.cards;
  },
  methods: {
    handleChangeCard(el) {
      let index = 0
      if ('added' in el) {
        index = el.added.newIndex
        let card = this.tickets[index]

        console.log('new pipeline id', this.pipelineId)
        console.log('card id', card.id)

        // continue from here.
        // send pipeline id & card id in request

        this.$axios.post('http://localhost:8000/api/pipelines', {
          pipeline_id: this.pipelineId,
          card_id: card.id
        }).then(response => {
          this.pipelines = response.data.data
        }).catch(err => {
          console.log(err)
        })
      }
    }
  },
}
</script>
