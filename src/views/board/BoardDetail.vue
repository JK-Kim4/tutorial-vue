<template>
  <div class="board-detail">
    <div class="common-buttons">
      <button
        type="button"
        class="w3-button w3-round w3-blue-gray"
        v-on:click="fnUpdate"
      >
        수정
      </button>
      <button
        type="button"
        class="w3-button w3-round w3-blue-red"
        v-on:click="fnDelete"
      >
        삭제
      </button>
      <button
        type="button"
        class="w3-button w3-round w3-blue-gray"
        v-on:click="fnList"
      >
        목록
      </button>
    </div>

    <div class="board-contents">
      <h3>{{ title }}</h3>
      <div>
        <strong class="w3-large">{{ author }}</strong>
        <br />
        <span>{{ created_at }}</span>
      </div>
    </div>

    <div class="board-contents">
      <span>{{ contents }}</span>
    </div>
    <div class="common-buttons">
      <button
        type="button"
        class="w3-button w3-round w3-blue-gray"
        v-on:click="fnUpdate"
      >
        수정
      </button>
      <button
        type="button"
        class="w3-button w3-round w3-blue-red"
        v-on:click="fnDelete"
      >
        삭제
      </button>
      <button
        type="button"
        class="w3-button w3-round w3-blue-gray"
        v-on:click="fnList"
      >
        목록
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      requestBody: this.$route.query,
      bookId: this.$route.bookId,

      title: '',
      author: '',
      contents: '',
      created_at: '',
    }
  },
  mounted() {
    this.fnGetView()
  },
  methods: {
    fnGetView() {
      this.$axios
        .get(this.$serverUrl + '/kr/api/book/' + this.bookId, {
          params: this.requestBody,
        })
        .then((result) => {
          console.log(result)
        })
        .catch((err) => {
          console.log(err)
        })
    },
    //목록 화면
    fnList() {
      delete this.requestBody.bookId
      this.$route.push({
        path: './list',
        query: this.requestBody,
      })
    },
    //수정 화면
    fnUpdate() {
      this.$route.push({
        path: './write',
        query: this.requestBody,
      })
    },
    fnDelete() {
      if (!confirm('정말 삭제하시겠습니까')) return

      this.$axios
        .delete(this.$serverUrl + '/delete/' + this.bookId, {})
        .then(() => {
          alert('삭제가 완료되었습니다.')
          this.fnList()
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>
<style scoped></style>
