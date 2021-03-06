<template>
  <div>
    <hr />
    <action-card :title="'村建てメニュー'" :exists-footer="false">
      <template v-slot:content>
        <div class="content has-text-left">
          <div v-if="situation.creator.available_creator_say" class="m-b-20">
            <p style="font-weight: 700; margin-bottom: 6px;">村建て発言</p>
            <creator-message-input v-model="message" ref="messageInput" />
            <div class="has-text-right">
              <b-button
                :disabled="!canSay"
                @click="sayConfirm"
                type="is-primary"
                size="is-small"
              >
                発言確認
              </b-button>
            </div>
            <modal-say
              :is-open="isSayModalOpen"
              :confirm-message="confirmMessage"
              :village="village"
              :situation="situation"
              @close="isSayModalOpen = false"
              @say="say"
            />
          </div>
          <div v-if="situation.creator.available_modify_setting" class="m-b-20">
            <p style="font-weight: 700; margin-bottom: 6px;">設定変更</p>
            <b-field>
              <p class="control has-text-right">
                <b-button
                  :to="{ path: '/setting', query: { id: village.id } }"
                  tag="nuxt-link"
                  type="is-primary"
                  size="is-small"
                >
                  村の設定を変更する
                </b-button>
              </p>
            </b-field>
          </div>
          <div v-if="situation.creator.available_kick" class="m-b-20">
            <p style="font-weight: 700; margin-bottom: 6px;">強制退村</p>
            <b-field>
              <b-select v-model="participantId" expanded size="is-small">
                <option
                  v-for="participant in participants"
                  :value="participant.id"
                  :key="participant.id"
                  >{{ participant.chara.chara_name.full_name }}</option
                >
              </b-select>
              <p class="control">
                <button class="button is-danger is-small" @click="kickConfirm">
                  強制退村確認
                </button>
              </p>
            </b-field>
          </div>
          <div v-if="situation.creator.available_cancel_village" class="m-b-20">
            <p style="font-weight: 700; margin-bottom: 6px;">廃村</p>
            <b-field>
              <p class="control has-text-right">
                <b-button
                  type="is-danger"
                  size="is-small"
                  @click="cancelVillageConfirm"
                >
                  廃村確認
                </b-button>
              </p>
            </b-field>
          </div>
        </div>
      </template>
      <template v-slot:footer> </template>
    </action-card>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'nuxt-property-decorator'
// component
import actionCard from '~/components/village/action/action-card.vue'
import creatorMessageInput from '~/components/village/action/creator/creator-message-input.vue'
// type
import Village from '~/components/type/village'
import VillageParticipant from '~/components/type/village-participant'
import SituationAsParticipant from '~/components/type/situation-as-participant'
import VillageCreateSituation from '~/components/type/village-creator-situation'
import Message from '~/components/type/message'
import { VILLAGE_STATUS } from '~/components/const/consts'
import toast from '~/components/village/village-toast'
const modalSay = () => import('~/components/village/action/say/modal-say.vue')

@Component({
  components: { actionCard, creatorMessageInput, modalSay }
})
export default class VillageCreator extends Vue {
  // ----------------------------------------------------------------
  // props
  // ----------------------------------------------------------------
  @Prop({ type: Object })
  private village!: Village

  @Prop({ type: Object })
  private situation!: SituationAsParticipant

  // ----------------------------------------------------------------
  // data
  // ----------------------------------------------------------------
  private participantId: number = this.participants[0].id
  private message: string = ''
  private isSayModalOpen: boolean = false
  private confirmMessage: Message | null = null

  // ----------------------------------------------------------------
  // computed
  // ----------------------------------------------------------------
  private get participants(): VillageParticipant[] {
    return this.village.participant.member_list.concat(
      this.village.spectator.member_list
    )
  }

  private get isOver(): boolean {
    // @ts-ignore
    return this.$refs.messageInput.existsOver
  }

  private get canSay(): boolean {
    if (this.message == null || this.message.trim() === '') return false
    if (this.isOver) return false
    return true
  }

  // ----------------------------------------------------------------
  // methods
  // ----------------------------------------------------------------
  private kickConfirm(): void {
    const self = this
    this.$buefy.dialog.confirm({
      title: '強制退村確認',
      message: '本当に強制退村させますか？',
      confirmText: '退村させる',
      type: 'is-danger',
      hasIcon: true,
      iconPack: 'fas',
      onConfirm: async () => {
        await self.kick()
        toast.info(self, '退村させました')
      },
      size: 'is-small',
      cancelText: 'キャンセル'
    })
  }

  private async kick(): Promise<void> {
    try {
      await this.$axios.$post(`/creator/village/${this.village.id}/kick`, {
        target_id: this.participantId
      })
      this.$emit('reload')
    } catch (error) {}
  }

  private cancelVillageConfirm(): void {
    const self = this
    this.$buefy.dialog.confirm({
      title: '廃村確認',
      message: '本当に廃村にしますか？',
      confirmText: '廃村にする',
      type: 'is-danger',
      hasIcon: true,
      iconPack: 'fas',
      onConfirm: async () => {
        await self.cancelVillage()
        toast.info(self, '廃村にしました')
      },
      size: 'is-small',
      cancelText: 'キャンセル'
    })
  }

  private async cancelVillage(): Promise<void> {
    try {
      await this.$axios.$post(`/creator/village/${this.village.id}/cancel`)
      this.$emit('reload')
    } catch (error) {}
  }

  private async sayConfirm(): Promise<void> {
    try {
      this.confirmMessage = await this.$axios.$post(
        `/creator/village/${this.village.id}/say-confirm`,
        {
          message: this.message
        }
      )
      this.isSayModalOpen = true
    } catch (error) {
      toast.danger(this, '発言確認失敗')
    }
  }

  private async say(): Promise<void> {
    try {
      await this.$axios.$post(`/creator/village/${this.village.id}/say`, {
        message: this.message
      })
      this.message = ''
    } catch (error) {
      toast.danger(this, '発言失敗')
    }
    await this.$emit('reload')
  }
}
</script>
