<template>
  <div class="w-full cursor-pointer grid" v-click-away="onClickAway">
    <textarea
      class="resize-none w-full h-16 px-2 py-1 text-sm outline-none rounded border-b border-gray7"
      data-cy="new-card-input"
      v-model="cardTitle"
      v-on:keydown.enter.prevent="addCard"
      v-on:keyup.esc.prevent="
        this.$emit('toggleInput', false);
        cardTitle = '';
      "
      placeholder="Enter a title for this card..."
      ref="cardCreate"
    />
    <div>
      <SaveButton buttonText="Add card" @click="addCard" />
      <Cross
        data-cy="cancel"
        class="w-8 h-8 p-1 mx-0.5 fill-current text-gray-600 order-last inline-block"
        @click.stop="
          this.$emit('toggleInput', false);
          cardTitle = '';
        "
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { store } from '@/stores/store';
import Card from '@/typings/card';
import moment from 'moment';
import SaveButton from '@/components/SaveButton.vue';
import Cross from '@/assets/icons/cross.svg';

export default defineComponent({
  setup() {
    const state = store();
    const createCard = state.createCard;
    return { state, createCard };
  },
  data() {
    return {
      cardTitle: ''
    };
  },
  mounted() {
    const cardInput = this.$refs.cardCreate as HTMLElement;
    cardInput.focus();
  },
  components: {
    SaveButton,
    Cross
  },
  props: ['list'],
  emits: ['toggleInput'],
  methods: {
    onClickAway() {
      this.$emit('toggleInput', false);
      this.cardTitle = '';
    },
    addCard() {
      if (!this.cardTitle) {
        return;
      }

      this.createCard({
        boardId: this.state.board.id,
        listId: this.list.id,
        name: this.cardTitle
      });
      this.cardTitle = '';
    }
  }
});
</script>

<style></style>
