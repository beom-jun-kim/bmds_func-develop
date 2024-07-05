<template>
  <div class="step-container">
    <h2>STEP 01 : 기회탐색</h2>
    <h3>1-3 : 아이템 해결 가능성 확인</h3>
    <p>3. 고객의 입장에서 불만요소(Pain Point) 개선에 대한 충분한 비용 지불 의사를 확인합니다.</p>
    <p>3-1. 고객의 불편함과 요구사항 그리고 불 충족 니즈 가운데에 우리가 해결 할 수 있는 문제는 무엇일까요?</p>
    <div class="table-container">
      <div class="table">
        <div class="table-row">
          <div class="table-cell header">고객의 불편함 요소</div>
          <div class="table-cell header">경쟁사 제품의 단점</div>
        </div>
        <div class="table-row">
          <div class="table-cell">
            <label for="customer-issue-1">1순위</label>
            <input
              type="text"
              id="customer-issue-1"
              v-model="customerIssues[0]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('customerIssues', 0)"
            />
          </div>
          <div class="table-cell">
            <label for="competitor-flaw-1">1순위</label>
            <input
              type="text"
              id="competitor-flaw-1"
              v-model="competitorFlaws[0]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('competitorFlaws', 0)"
            />
          </div>
        </div>
        <div class="table-row">
          <div class="table-cell">
            <label for="customer-issue-2">2순위</label>
            <input
              type="text"
              id="customer-issue-2"
              v-model="customerIssues[1]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('customerIssues', 1)"
            />
          </div>
          <div class="table-cell">
            <label for="competitor-flaw-2">2순위</label>
            <input
              type="text"
              id="competitor-flaw-2"
              v-model="competitorFlaws[1]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('competitorFlaws', 1)"
            />
          </div>
        </div>
        <div class="table-row">
          <div class="table-cell">
            <label for="customer-issue-3">3순위</label>
            <input
              type="text"
              id="customer-issue-3"
              v-model="customerIssues[2]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('customerIssues', 2)"
            />
          </div>
          <div class="table-cell">
            <label for="competitor-flaw-3">3순위</label>
            <input
              type="text"
              id="competitor-flaw-3"
              v-model="competitorFlaws[2]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('competitorFlaws', 2)"
            />
          </div>
        </div>
        <div class="table-row">
          <div class="table-cell">
            <label for="customer-issue-4">4후보</label>
            <input
              type="text"
              id="customer-issue-4"
              v-model="customerIssues[3]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('customerIssues', 3)"
            />
          </div>
          <div class="table-cell">
            <label for="competitor-flaw-4">4후보</label>
            <input
              type="text"
              id="competitor-flaw-4"
              v-model="competitorFlaws[3]"
              :readonly="isConnecting"
              :class="{ 'connecting-outline': isConnecting }"
              placeholder="작성해 주세요."
              @click="isConnecting && selectIssue('competitorFlaws', 3)"
            />
          </div>
        </div>
      </div>
    </div>
    <div class="buttons">
      <button @click="startEditing">수정하기</button>
      <button @click="startConnecting">연결하기</button>
      <button @click="resetConnections">초기화</button>
    </div>
    <p v-if="isConnecting">두 박스를 연결하여 주세요!</p>
    <p>3-2. 해결이 될 경우 고객이 우리 제품 또는 서비스를 구매할 가능성은 충분히 있을까요?</p>
    <div class="options">
      <label>
        <input type="radio" name="purchase-possibility" v-model="purchasePossibility" value="unknown" />
        가. 잘 모르겠어요
      </label>
      <label>
        <input type="radio" name="purchase-possibility" v-model="purchasePossibility" value="likely" />
        나. 그럴 수 있을 것 같아요
      </label>
      <label>
        <input type="radio" name="purchase-possibility" v-model="purchasePossibility" value="definitely" />
        다. 충분히 그럴거예요
      </label>
    </div>
    <div class="connections">
      <h3>결과:</h3>
      <ul>
        <li v-for="(connection, index) in sortedConnections" :key="connection.id">
          {{ index + 1 }}순위 {{getText(connection.sourceType, connection.sourceIndex) }} , {{ getText(connection.targetType, connection.targetIndex) }}
          <br>
          결과: {{ connection.result }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'StepComponent',
  data() {
    return {
      customerIssues: ['', '', '', ''],
      competitorFlaws: ['', '', '', ''],
      purchasePossibility: '',
      selectedSource: null,
      selectedTarget: null,
      connections: [],
      isConnecting: false
    };
  },
  computed: {
    sortedConnections() {
      return this.connections
        .map(connection => ({
          ...connection,
          result: this.calculateResult(connection)
        }))
        .sort((a, b) => b.result - a.result);
    }
  },
  methods: {
    startConnecting() {
      this.isConnecting = true;
      this.clearSelection();
    },
    startEditing() {
      this.isConnecting = false;
      this.clearSelection();
    },
    resetConnections() {
      this.connections = [];
      this.clearSelection();
    },
    selectIssue(type, index) {
      if (this.isConnecting) {
        if (!this.selectedSource) {
          if (this.isEmptyBox(type, index)) {
            alert('빈 상자는 선택할 수 없습니다. 텍스트를 작성해 주세요.');
          } else {
            this.selectedSource = { type, index };
          }
        } else {
          if (this.selectedSource.type === type) {
            alert('같은 타입의 박스를 선택할 수 없습니다.');
          } else if (this.isEmptyBox(type, index)) {
            alert('빈 상자끼리 연결할 수 없습니다. 텍스트를 작성해 주세요.');
          } else {
            this.selectedTarget = { type, index };
            this.connectIssues();
          }
        }
      }
    },
    clearSelection() {
      this.selectedSource = null;
      this.selectedTarget = null;
    },
    connectIssues() {
      if (this.selectedSource && this.selectedTarget) {
        this.connections.push({
          id: Date.now(),
          sourceType: this.selectedSource.type,
          sourceIndex: this.selectedSource.index,
          targetType: this.selectedTarget.type,
          targetIndex: this.selectedTarget.index
        });
        this.clearSelection();
      }
    },
    isEmptyBox(type, index) {
      return (
        (type === 'customerIssues' && this.customerIssues[index] === '') ||
        (type === 'competitorFlaws' && this.competitorFlaws[index] === '')
      );
    },
    getWeight(index) {
      switch (index) {
        case 0:
          return 0.95;
        case 1:
          return 0.90;
        case 2:
          return 0.85;
        case 3:
          return 0.80;
        default:
          return 0;
      }
    },
    calculateResult(connection) {
      const sourceWeight = this.getWeight(connection.sourceIndex);
      const targetWeight = this.getWeight(connection.targetIndex);
      return (sourceWeight * targetWeight * 100).toFixed(2);
    },
    getText(type, index) {
      return type === 'customerIssues' ? this.customerIssues[index] : this.competitorFlaws[index];
    }
  }
};
</script>

<style scoped>
.step-container {
  font-family: Arial, sans-serif;
}

h2 {
  color: #ff8c00;
}

h3 {
  color: #ff8c00;
}

.table-container {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

.table {
  width: 100%;
  max-width: 800px;
  border-collapse: collapse;
}

.table-row {
  display: flex;
}

.table-cell {
  flex: 1;
  border: 1px solid #ddd;
  padding: 8px;
}

.header {
  background-color: #ff8c00;
  color: white;
  text-align: center;
}

.table-cell input {
  width: 100%;
  padding: 6px;
  margin-top: 6px;
  box-sizing: border-box;
  cursor: auto;
}

.table-cell input.connecting-outline {
  border: 2px solid #ff8c00;
  cursor: pointer;
}

.buttons {
  margin-top: 20px;
}

.buttons button {
  margin-right: 10px;
}

.options {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-top: 20px;
}

.options label {
  margin-bottom: 10px;
}

.connections {
  margin-top: 20px;
}

.connections ul {
  list-style-type: none;
  padding: 0;
}

.connections li {
  margin: 5px 0;
}
</style>
