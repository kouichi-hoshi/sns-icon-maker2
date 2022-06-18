<script>
import { ref, defineComponent } from 'vue'
import svgIcon from '../components/svgIcon.vue'

export default defineComponent({
  name: 'imgPreview',
  props: {
    path: {
      type: Object,
      required: true,
    },
    faceParts: {
      type: Array,
      required: true,
    },
    typeNames: {
      type: Array,
      required: true,
    },
    faceTag: {
      type: String,
      required: true,
      default: 'girl',
    },
    color: {
      type: String,
      required: true,
      default: 'black',
    },
  },
  components: {
    svgIcon,
  },
  setup(props) {
    const path = props.path
    const faceParts = props.faceParts
    const faceTag = props.faceTag
    const typeNames = props.typeNames
    let color = props.color

    /**
     * 顔パーツをタグで絞り込む
     * @param {Array} faceParts 顔パーツオブジェクト
     * @param {String} tag 絞り込みたいタグ（girl、boy、など）
     */
    function findFacePartsByTag() {
      let items = []
      faceParts.forEach((part) => {
        part.tag.filter((tag) => {
          let bool = tag === faceTag
          if (bool) {
            items.push(part)
          }
        })
      })
      return items
    }

    /**
     * 顔パーツを顔タイプ（種類）ごとに配列にセットする
     * @param {Array} typeNames 顔パーツ名
     * @param {Array} faceParts 顔パーツオブジェクト（findFacePartsByTag()で処理されたオブジェクト）
     */
    function createFacePartObjByType(faceParts) {
      const parts = {}
      typeNames.forEach((typeName) => {
        parts[typeName] = []
        for (let facePart of faceParts) {
          if (facePart.type === typeName) {
            parts[typeName].push(facePart)
          }
        }
      })
      return parts
    }

    /**
     * 引数で指定した顔バーツを配列にセットして返す
     * @param {Array} faceParts tagで絞り込こまれ、タイプごとに顔パーツがセットされた配列を渡す（filterFaceItems()とtypeFaceItems()を実行後の配列）
     * @param {Array} typeNames'~/model/names.js'からimportしたtypeNamesを渡す
     * @param {Number} index 任意の数値を渡す  通常は0である想定
     */
    function setActiveFaceParts(faceParts, index = 0) {
      const activeParts = {}
      for (let type of typeNames) {
        activeParts[type] = faceParts[type][index]
      }
      return activeParts
    }

    //顔パーツの初期設定
    const activeFaceParts = setActiveFaceParts(
      createFacePartObjByType(findFacePartsByTag())
    )

    return {
      path,
      activeFaceParts,
    }
  },
})
</script>
<template>
  <div class="parts-preview mx-auto">
    <div
      class="parts-preview__img"
      v-bind:class="'parts-preview__img-' + key"
      v-for="(parts, key) of activeFaceParts"
      v-bind:key="key"
    >
      <svgIcon :name="parts.fileName" :color="color" />
    </div>
  </div>
</template>

<style lang="scss" scooped>
.parts-preview {
  position: relative;
  width: 320px;
  &__img {
    position: absolute;
    top: 0;
    left: 0;
  }
}
</style>
