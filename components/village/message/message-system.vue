<template>
  <div class="card-content m-b-5" :class="messageClass">
    <div class="content has-text-left">
      <message-text
        v-if="message.content.type.code !== 'PARTICIPANTS'"
        :message-text="message.content.text"
      />
      <message-participant-list
        v-if="message.content.type.code === 'PARTICIPANTS'"
        :village="village"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'nuxt-property-decorator'
import messageText from '~/components/village/message/message-text.vue'
import messageParticipantList from '~/components/village/message/message-participant-list.vue'
import Village from '~/components/type/village'
import Message from '~/components/type/message'
import { MESSAGE_TYPE } from '~/components/const/consts'

@Component({
  components: {
    messageText,
    messageParticipantList
  }
})
export default class SystemMessage extends Vue {
  @Prop({ type: Object })
  private message!: Message

  @Prop({ type: Object })
  private village!: Village

  private get messageClass(): string {
    switch (this.message.content.type.code) {
      case MESSAGE_TYPE.PUBLIC_SYSTEM:
        return ''
      case MESSAGE_TYPE.PRIVATE_SYSTEM:
        return 'message-system-private'
      case MESSAGE_TYPE.PRIVATE_SEER:
      case MESSAGE_TYPE.PRIVATE_WISE:
        return 'message-system-private-seer'
      case MESSAGE_TYPE.PRIVATE_PSYCHIC:
      case MESSAGE_TYPE.PRIVATE_GURU:
      case MESSAGE_TYPE.PRIVATE_CORONER:
        return 'message-system-private-psychic'
      case MESSAGE_TYPE.PRIVATE_WEREWOLF:
      case MESSAGE_TYPE.PRIVATE_FANATIC:
        return 'message-system-private-werewolf'
      case MESSAGE_TYPE.PRIVATE_MASON:
      case MESSAGE_TYPE.PRIVATE_SYMPATHIZER:
        return 'message-system-private-mason'
      case MESSAGE_TYPE.CREATOR_SAY:
        return 'message-system-creator'
      case MESSAGE_TYPE.PARTICIPANTS:
        return ''
      default:
        return ''
    }
  }
}
</script>

<style lang="scss" scoped>
.card-content {
  padding: 10px !important;

  &.message-system-private {
    border-top: 1px solid $private-system-border;
    border-bottom: 1px solid $private-system-border;
    background-color: $private-system-bg !important;
  }
  &.message-system-private-seer {
    border-top: 1px solid $seer-system-border;
    border-bottom: 1px solid $seer-system-border;
    background-color: $seer-system-bg !important;
  }
  &.message-system-private-psychic {
    border-top: 1px solid $psychic-system-border;
    border-bottom: 1px solid $psychic-system-border;
    background-color: $psychic-system-bg !important;
  }
  &.message-system-private-werewolf {
    border-top: 1px solid $werewolf-system-border;
    border-bottom: 1px solid $werewolf-system-border;
    background-color: $werewolf-system-bg !important;
  }
  &.message-system-private-mason {
    border-top: 1px solid $mason-system-border;
    border-bottom: 1px solid $mason-system-border;
    background-color: $mason-system-bg !important;
  }
  &.message-system-creator {
    border-top: 1px solid $creator-say-border;
    border-bottom: 1px solid $creator-say-border;
    background-color: $creator-say-bg !important;
  }
}
</style>
