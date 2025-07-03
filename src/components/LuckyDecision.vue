<template>
  <div class="lucky-container">
    <h1 class="title">🍀 運まかせ決定マシン 🍀</h1>
    <p class="subtitle">ENOさん、今日も運に任せてみませんか？</p>
    
    <div class="categories">
      <div 
        v-for="category in categories" 
        :key="category.id"
        class="category-card"
        @click="selectCategory(category)"
        :class="{ active: selectedCategory?.id === category.id }"
      >
        <div class="category-icon">{{ category.icon }}</div>
        <h3>{{ category.name }}</h3>
        <p>{{ category.description }}</p>
      </div>
    </div>

    <div v-if="selectedCategory" class="decision-area">
      <div v-if="selectedCategory.id === 'custom'" class="custom-input">
        <h3>新しい選択肢を追加</h3>
        <div class="add-option">
          <input 
            type="text" 
            v-model="newOption" 
            @keyup.enter="addOption"
            placeholder="例：新しい選択肢"
          >
          <button @click="addOption">追加</button>
        </div>
        <ul class="options-list">
          <li v-for="(option, index) in customOptions" :key="index">
            <span>{{ option }}</span>
            <button @click="removeOption(index)" class="remove-btn">×</button>
          </li>
        </ul>
        <p v-if="customOptions.length === 0" class="no-options">選択肢を追加してください。</p>
      </div>
      
      <button 
        class="decision-button" 
        @click="makeDecision"
        :disabled="isDeciding"
      >
        {{ isDeciding ? '運命を決めています...' : '運に任せる！' }}
      </button>
    </div>

    <div v-if="result" class="result" :class="{ show: showResult }">
      <h2>🎯 運の決定 🎯</h2>
      <div class="result-content">
        <div class="result-icon">{{ resultIcon }}</div>
        <div class="result-text">{{ result }}</div>
        <div class="result-message">{{ resultMessage }}</div>
      </div>
      <button class="again-button" @click="reset">もう一度運に任せる</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LuckyDecision',
  data() {
    return {
      selectedCategory: null,
      newOption: '',
      customOptions: [],
      result: '',
      resultIcon: '',
      resultMessage: '',
      isDeciding: false,
      showResult: false,
      categories: [
        {
          id: 'fortune',
          name: '今日の運勢',
          icon: '🔮',
          description: '今日一日の運勢を占います'
        },
        {
          id: 'food',
          name: '晩ご飯決め',
          icon: '🍽️',
          description: '今日の夕食メニューを決めます'
        },
        {
          id: 'yesno',
          name: 'Yes/No判定',
          icon: '⚖️',
          description: '迷った時の二択判定'
        },
        {
          id: 'color',
          name: 'ラッキーカラー',
          icon: '🌈',
          description: '今日のラッキーカラーを選びます'
        },
        {
          id: 'activity',
          name: '今日やること',
          icon: '⚡',
          description: '今日のおすすめ活動を提案'
        },
        {
          id: 'custom',
          name: 'カスタム選択',
          icon: '🎲',
          description: '自分で選択肢を決めて運に任せる'
        },
        {
          id: 'lucky-number',
          name: 'ラッキーナンバー',
          icon: '🔢',
          description: '今日のラッキーナンバーを引く'
        }
      ],
      fortunes: [
        { result: '大吉', message: '今日は最高の一日になりそうです！何事にも積極的に！', icon: '🌟' },
        { result: '中吉', message: '良いことが起こりそうな予感！チャンスを逃さずに！', icon: '✨' },
        { result: '小吉', message: '小さな幸せが見つかりそう！身近なことに目を向けて！', icon: '🍀' },
        { result: '吉', message: '安定した一日になりそう！いつも通りで大丈夫！', icon: '😊' },
        { result: '末吉', message: '後半に良いことがありそう！最後まで諦めずに！', icon: '🌅' },
        { result: '凶', message: '今日は慎重に！でも明日はきっと良い日！', icon: '🛡️' }
      ],
      foods: [
        '寿司', 'ラーメン', 'カレー', 'パスタ', 'ハンバーガー', '焼肉', '天ぷら', 'そば', 'うどん', 
        'ピザ', 'オムライス', 'チャーハン', '親子丼', 'とんかつ', '刺身', '鍋料理', '野菜炒め', 
        'サラダ', 'サンドイッチ', 'おにぎり', 'お弁当', 'パン', 'お好み焼き', 'たこ焼き', '餃子'
      ],
      colors: [
        { name: '赤', message: '情熱とエネルギーの色！積極的に行動を！', icon: '❤️' },
        { name: '青', message: '冷静と安らぎの色！落ち着いて物事に取り組んで！', icon: '💙' },
        { name: '緑', message: '成長と調和の色！自然体でいることが大切！', icon: '💚' },
        { name: '黄色', message: '明るさと希望の色！ポジティブに過ごして！', icon: '💛' },
        { name: '紫', message: '神秘と創造の色！直感を大切に！', icon: '💜' },
        { name: 'オレンジ', message: '活力と社交の色！人との繋がりを大切に！', icon: '🧡' },
        { name: 'ピンク', message: '愛と優しさの色！思いやりを持って！', icon: '💗' },
        { name: '白', message: '純粋と新しい始まりの色！リセットの時！', icon: '🤍' }
      ],
      activities: [
        '散歩する', '音楽を聴く', '映画を見る', '本を読む', '料理する', '掃除する', '友達に連絡する',
        '新しいことを学ぶ', '運動する', '写真を撮る', 'ブログを書く', '瞑想する', 'ゲームをする',
        '絵を描く', '買い物に行く', 'カフェに行く', '温泉に入る', '早寝する', 'ストレッチする',
        'プログラミングする', 'DIYに挑戦する', '植物を育てる', '日記を書く', 'ヨガをする'
      ]
    }
  },
  methods: {
    selectCategory(category) {
      this.selectedCategory = category
      this.result = ''
      this.showResult = false
    },
    async makeDecision() {
      this.isDeciding = true
      this.showResult = false
      
      await new Promise(resolve => setTimeout(resolve, 1500))
      
      switch (this.selectedCategory.id) {
        case 'fortune':
          this.decideFortune()
          break
        case 'food':
          this.decideFood()
          break
        case 'yesno':
          this.decideYesNo()
          break
        case 'color':
          this.decideColor()
          break
        case 'activity':
          this.decideActivity()
          break
        case 'custom':
          this.decideCustom()
          break
        case 'lucky-number':
          this.decideLuckyNumber()
          break
      }
      
      this.isDeciding = false
      this.showResult = true
    },
    decideFortune() {
      const fortune = this.getRandomItem(this.fortunes)
      this.result = fortune.result
      this.resultMessage = fortune.message
      this.resultIcon = fortune.icon
    },
    decideFood() {
      const food = this.getRandomItem(this.foods)
      this.result = food
      this.resultMessage = `今日の夕食は${food}に決定！美味しく食べてくださいね！`
      this.resultIcon = '🍽️'
    },
    decideYesNo() {
      const decision = Math.random() < 0.5 ? 'YES' : 'NO'
      this.result = decision
      this.resultMessage = decision === 'YES' ? 'やってみましょう！きっと良い結果が！' : '今回は見送りが良さそう！別の機会に！'
      this.resultIcon = decision === 'YES' ? '✅' : '❌'
    },
    decideColor() {
      const color = this.getRandomItem(this.colors)
      this.result = color.name
      this.resultMessage = color.message
      this.resultIcon = color.icon
    },
    decideActivity() {
      const activity = this.getRandomItem(this.activities)
      this.result = activity
      this.resultMessage = `今日は${activity}をして過ごしてみてください！`
      this.resultIcon = '⚡'
    },
    decideCustom() {
      if (this.customOptions.length === 0) {
        this.result = 'エラー'
        this.resultMessage = '選択肢を追加してください！'
        this.resultIcon = '❗'
        return
      }
      const choice = this.getRandomItem(this.customOptions)
      this.result = choice
      this.resultMessage = `運命の選択は「${choice}」です！`
      this.resultIcon = '🎲'
    },
    decideLuckyNumber() {
      const number = Math.floor(Math.random() * 100) + 1
      this.result = number
      this.resultMessage = `今日のあなたのラッキーナンバーは「${number}」です！`
      this.resultIcon = '🔢'
    },
    addOption() {
      if (this.newOption.trim() !== '') {
        this.customOptions.push(this.newOption.trim())
        this.newOption = ''
      }
    },
    removeOption(index) {
      this.customOptions.splice(index, 1)
    },
    getRandomItem(array) {
      return array[Math.floor(Math.random() * array.length)]
    },
    reset() {
      this.selectedCategory = null
      this.customOptions = []
      this.result = ''
      this.showResult = false
    }
  }
}
</script>

<style scoped>
.lucky-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  color: white;
  text-align: center;
}

.title {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.subtitle {
  font-size: 1.2em;
  margin-bottom: 30px;
  opacity: 0.9;
}

.categories {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.category-card {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  border: 2px solid transparent;
}

.category-card:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-5px);
}

.category-card.active {
  border-color: #FFD700;
  background: rgba(255, 215, 0, 0.2);
}

.category-icon {
  font-size: 3em;
  margin-bottom: 10px;
}

.category-card h3 {
  margin: 10px 0 5px 0;
  font-size: 1.3em;
}

.category-card p {
  margin: 0;
  opacity: 0.8;
  font-size: 0.9em;
}

.decision-area {
  margin: 30px 0;
}

.custom-input {
  margin-bottom: 20px;
}

.custom-input h3 {
  margin-bottom: 10px;
}

.custom-input textarea {
  width: 100%;
  max-width: 400px;
  padding: 10px;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.9);
  color: #333;
  font-size: 1em;
  resize: vertical;
}

.add-option {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 10px;
}

.add-option input {
  flex-grow: 1;
  max-width: 300px;
  padding: 10px;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.9);
  color: #333;
  font-size: 1em;
}

.add-option button {
  padding: 10px 20px;
  border: none;
  border-radius: 10px;
  background: #FFD700;
  color: #333;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s;
}

.add-option button:hover {
  background: #FFA500;
}

.options-list {
  list-style: none;
  padding: 0;
  margin: 0 auto 20px;
  max-width: 400px;
  max-height: 150px;
  overflow-y: auto;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 10px;
}

.options-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255,255,255,0.1);
  padding: 8px 15px;
  border-radius: 8px;
  margin-bottom: 5px;
}

.remove-btn {
  background: transparent;
  border: none;
  color: white;
  font-size: 1.2em;
  cursor: pointer;
  padding: 0 5px;
}

.no-options {
  opacity: 0.7;
}

.decision-button {
  background: linear-gradient(45deg, #FFD700, #FFA500);
  color: #333;
  border: none;
  padding: 15px 30px;
  font-size: 1.2em;
  border-radius: 25px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  box-shadow: 0 4px 15px rgba(255, 215, 0, 0.3);
}

.decision-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 215, 0, 0.4);
}

.decision-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.result {
  background: rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  padding: 30px;
  margin-top: 30px;
  backdrop-filter: blur(15px);
  border: 2px solid rgba(255, 215, 0, 0.3);
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.5s ease;
}

.result.show {
  opacity: 1;
  transform: translateY(0);
}

.result h2 {
  margin-top: 0;
  font-size: 1.8em;
  color: #FFD700;
}

.result-content {
  margin: 20px 0;
}

.result-icon {
  font-size: 4em;
  margin-bottom: 15px;
}

.result-text {
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 10px;
  color: #FFD700;
}

.result-message {
  font-size: 1.1em;
  line-height: 1.6;
  opacity: 0.9;
}

.again-button {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.3);
  padding: 10px 20px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 20px;
}

.again-button:hover {
  background: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.5);
}

@media (max-width: 768px) {
  .title {
    font-size: 2em;
  }
  
  .categories {
    grid-template-columns: 1fr;
  }
  
  .category-card {
    padding: 15px;
  }
  
  .result-text {
    font-size: 1.5em;
  }
}
</style> 