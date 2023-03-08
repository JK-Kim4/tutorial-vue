<template>
  <div class="board-list">
    <div class="common-buttons">
      <button
        type="button"
        class="w3-button w3-round w3-blue-gray"
        v-on:click="fnWrite"
      >
        등록
      </button>
    </div>
    <table class="w3-table-all">
      <thead>
        <tr>
          <th>id</th>
          <th>제목</th>
          <th>저자</th>
          <th>분류</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(value, key) in list" :key="key">
          <td>{{ value.bookId }}</td>
          <td>{{ value.bookTitle }}</td>
          <td>
            <span v-for="(value2, key2) in value.authorList" :key="key2">
              {{ value2.authorKrNm }},
            </span>
          </td>
          <td>
            <span v-for="(value3, key3) in value.categoryList" :key="key3">
              {{ value3.metaValue }}
            </span>
          </td>
        </tr>
      </tbody>
    </table>
    <div
      class="pagination w3-bar w3-padding-16 w3-small"
      v-if="paging.total_list_cnt > 0"
    >
      <span class="pg">
        <a
          href="javascript:;"
          @click="fnPage(1)"
          class="first w3-button w3-bar-item w3-border"
          >&lt;&lt;</a
        >
        <a
          href="javascript:;"
          v-if="paging.start_page > 10"
          @click="fnPage(`${paging.start_page - 1}`)"
          class="prev w3-button w3-bar-item w3-border"
          >&lt;</a
        >
        <template v-for="(n, index) in paginavigation()">
          <template v-if="paging.page == n">
            <strong
              class="w3-button w3-bar-item w3-border w3-green"
              :key="index"
              >{{ n }}</strong
            >
          </template>
          <template v-else>
            <a
              class="w3-button w3-bar-item w3-border"
              href="javascript:;"
              @click="fnPage(`${n}`)"
              :key="index"
              >{{ n }}</a
            >
          </template>
        </template>
        <a
          href="javascript:;"
          v-if="paging.total_page_cnt > paging.end_page"
          @click="fnPage(`${paging.end_page + 1}`)"
          class="next w3-button w3-bar-item w3-border"
          >&gt;</a
        >
        <a
          href="javascript:;"
          @click="fnPage(`${paging.total_page_cnt}`)"
          class="last w3-button w3-bar-item w3-border"
          >&gt;&gt;</a
        >
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    //변수생성
    return {
      requestBody: {}, //리스트 페이지 데이터전송
      list: {}, //리스트 데이터
      paging: {}, //페이징 데이터
      page: this.$route.query.page ? this.$route.query.page : 1,
      size: this.$route.query.size ? this.$route.query.size : 10,
      keyword: this.$route.query.keyword,
      paginavigation: function () {
        //페이징 처리 for문 커스텀
        let pageNumber = [] //;
        let start_page = this.paging.start_page
        let end_page = this.paging.end_page
        for (let i = start_page; i <= end_page; i++) pageNumber.push(i)
        return pageNumber
      },
    }
  },
  mounted() {
    this.fnGetList()
  },
  methods: {
    fnGetList() {
      this.$axios
        .get(this.$serverUrl + '/kr/api/books?pageNum=1&pageSize=10', {})
        .then((res) => {
          this.list = res.data.data.list
          this.paging = res.data.data
          console.log(this.list)
          console.log(this.paging)
        })
        .catch((err) => {
          if (err.message.indexOf('Network Error') > -1) {
            alert('네트워크가 원활하지 않습니다.\n잠시 후 다시 시도해주세요.')
          }
        })
    },
  },
}
</script>
