<template>
  <section class="section">
    <div class="container">
      <h1 class="title is-5">更新情報</h1>
      <div class="content">
        <ul class="has-text-left is-size-7">
          <li>2020/08/13 役職：賢者を追加</li>
          <li>2020/08/13 役職：導師を追加</li>
          <li>2020/08/13 役職：検死官を追加</li>
          <li>2020/08/13 役職：パン屋を追加</li>
          <li>2020/08/13 役職：呪狼を追加</li>
          <li>2020/08/13 役職：智狼を追加</li>
          <li>2020/08/13 役職：共鳴者を追加</li>
          <li>2020/08/13 役職：狂信者を追加</li>
          <li>
            2020/08/11
            発言時にサーバーエラーが起きた際発言内容が消失する不具合を修正
          </li>
          <li>2020/08/11 囁きのアンカーコピーはキャラ略称をつけないよう変更</li>
          <li>2020/08/11 役職：聖痕者を追加</li>
          <li>2020/08/11 役職：猫又を追加</li>
          <li>2020/08/09 発言誤爆防止機能を追加</li>
          <li>2020/08/09 カミングアウト機能を追加</li>
          <li>2020/08/06 プレイヤーによる村作成機能を追加</li>
          <li>2020/07/12 村設定：ダミー役欠けを追加</li>
          <li>2020/07/11 参加者一覧から個別抽出できるよう変更</li>
          <li>2020/07/11 発言抽出のシステムメッセージを公開と非公開に分離</li>
          <li>2020/07/11 下部メニューのタブや大きさを記憶するよう変更</li>
          <li>2020/07/11 アンカーコピーにキャラ略称を追加</li>
          <li>2020/07/11 突然死した場合陣営勝利しても敗北扱いとするよう変更</li>
          <li>2020/06/08 役職：妖狐を追加</li>
          <li>2020/06/03 役職：共有者を追加</li>
          <li>2020/06/02 役職：C国狂人を追加</li>
          <li>2020/06/02 本番サーバ公開</li>
          <li>2020/02/10 テストサーバ公開</li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
// component
// type

@Component({
  components: {}
})
export default class extends Vue {
  /** head */
  private head() {
    return { title: ' | 更新情報' }
  }
}
</script>

<style lang="scss" scoped></style>
