<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'imgPreview',
  props: {
    path: {
      type: Object,
      required: true
    },
    faceParts: {
      type: Array,
      required: true
    },
    typeNames: {
      type: Array,
      required: true
    },
    faceTag: {
      type: String,
      required: true,
      default: 'girl'
    },
  },
  components: {
    SVGElement,
  },
  setup (props) {

    const path = props.path
    const faceParts = props.faceParts
    const faceTag = props.faceTag
    const typeNames = props.typeNames
    let item = ref("aaa")

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

    // console.log(
    //   setActiveFaceParts(
    //     createFacePartObjByType(
    //       findFacePartsByTag()
    //     ), 1
    //   )
    // )
    const activeFaceParts = setActiveFaceParts( createFacePartObjByType( findFacePartsByTag() ) )

    return {
      path,
      item,
      activeFaceParts
    }
  }
}

</script>
<template>

<hr>

        <div class="part-preview">

          <!-- face parts preview -->
          <div class="part-preview__inner">

            <!-- face parts hair-back -->
            <!-- face parts hair-back end -->

            <!-- face parts -->
            <div class="part-preview__img test"
              v-bind:class="'part-preview__img-' + itemKey"
              v-for="(item, itemKey) of activeFaceParts"
              v-bind:key="itemKey"
            >
            <svg viewBox="0 0 640 640">
              <use :href="path.faceParts + item.fileName + '.svg' + '#' + item.fileName" />
            </svg>
            <img :src="path.faceParts + item.fileName + '.svg' + '#' + item.fileName" />
            </div><!-- face parts end -->

          </div><!-- face parts preview end -->

          <!-- capture preview -->
          <!-- capture preview end -->

        </div>

<hr>



</template>

<style lang="scss" scooped>
.part-preview {
  &__img {
    max-width: 640px;
  }
}
.part-preview__img.test {
  // border: 1px solid black;
  fill: currentColor;
}
</style>