<template>
  <div>
    <v-card>
      <v-card-title>Part6 - 1</v-card-title>
      <v-container>
        {{ data.preview }}
      </v-container>
    </v-card>
        
    <v-card class="mt-3">
      <v-tabs v-model="tabs">
        <v-tab v-for="(question, queId) in questions" :key="queId">
          {{ "Q" + (queId + 1) }}
        </v-tab>
      </v-tabs>

      <v-tabs-items v-model="tabs">
        <v-tab-item v-for="(question, queId_) in questions" :key="queId_">
          <v-list>
            <v-list-item
              v-for="(answer, answerId) in question.view_tree.children[2].children" :key="answerId"
              @click="selAnswer(answer.children[0].children[0].chunk_id)"
            >
              <v-list-item-avatar color="black">
                <span class="white--text">{{ answerId + 1 }}</span>
              </v-list-item-avatar>
              <v-list-item-content>
                <span>{{ chunckMap[answer.children[0].children[0].chunk_id].text_en }}</span>
                <!-- <div v-if=""></div> -->
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-tab-item>          
      </v-tabs-items>
      <!-- <v-btn @click=submitAnswer(questions)>h</v-btn>  -->
    </v-card>
    <!-- <div v-for="(item, index) in data.chunk_map" :key="index">
      <div v-if="index == 631165">
        {{ item.text_en.replace(/%d/gi, 1) }}
      </div>
      <div v-else>
        {{ item.text_en }}
      </div>
    </div> -->
  </div>
</template>

<script>

  export default {
    name: 'Home',
    data() {
      return {
        data: {},
        tabs: 0,
      }
    },
    computed: {
      questions() {
        return this.data.questions;
      },
      chunckMap() {
        return this.data.chunk_map
      }
    },
    created() {
      // 초기 실행 메소드 실행
      this.init();
    },
    methods: {
      // 초기 실행 메소드 정의
      init() {
        // 데이터 Fetch
        this.$axios.get('/task_container.json')
          .then(({ data }) => {
            this.data = data;
          })
      },
      // Answer 선택 시
      selAnswer(chunkId) {
        if(!(this.tabs === 3)) {
          this.tabs++;
        }
        this.$store.commit("selAnswer", {
         questionId: this.tabs, answerId: chunkId
        });
      },
      // Answers 최종 제출
      submitAnswer() {

      }
    }
  }
  </script>
