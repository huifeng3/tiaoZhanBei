<template>
    <div class="join">
        <div class="content-container">
            <!-- 左边模块框 -->
            <div class="left-content">
                <div class="title-bar">
                    社区动态
                </div>
                <div class="post-container">
                    <!-- 动态列表 -->
                    <div v-for="(post, index) in posts" :key="index" class="post">
                        <div class="post-header">
                            <img :src=post.avatar alt="User Avatar" class="post-avatar">
                            <div class="post-info">
                                <div class="post-nickname">{{ post.nickname }}</div>
                                <div class="post-timestamp">{{ post.timestamp }}</div>
                            </div>
                        </div>
                        <div class="post-content">
                            {{ post.content }}
                        </div>
                        <div class="post-actions">
                            <button class="action-button like-button">点赞</button>
                            <button class="action-button comment-button">评论</button>
                        </div>
                    </div>
                </div>
                <div class="input-container">
                    <input v-model="newPost" type="text" placeholder="友善发言，共享和谐社区...">
                    <button @click="addPost">发送</button>
                </div>
            </div>

            <!-- 右边模块框 -->
            <div class="right-content">
                <div class="notes notes-first">
                    <div class="title-bar">防诈知识问答</div>
                    <div class="quiz-container">
                        <div class="quiz-content">
                            <div class="question">
                                {{ currentQuestion.question }}
                            </div>
                            <div class="options">
                                <div v-for="(option, index) in currentQuestion.options" :key="index" class="option">
                                    <input type="radio" :id="'option' + index" :value="option" v-model="selectedOption">
                                    <label :for="'option' + index">{{ option }}</label>
                                </div>
                            </div>
                        </div>
                        <div class="quiz-explanation" v-if="showExplanation">
                            <div class="explanation-title">本题解析</div>
                            <div class="explanation-content">
                                {{ currentQuestion.explanation }}
                            </div>
                        </div>
                    </div>
                    <div class="quiz-actions">
                        <button class="action-button view-explanation" @click="showExplanation = true">查看解析</button>
                        <button class="action-button next-question" @click="nextQuestion">下一题</button>
                    </div>
                </div>
                <div class="notes notes-second">
                    <div class="title-bar">近期高频诈骗案例</div>
                    <div class="warn-container">
                        <div class="left-warn-content">
                            <div class="warn-title">
                                案例分析🔍
                            </div>
                            <strong>刷单诈骗</strong>

                            系统近期监测到多起刷单诈骗案件，犯罪分子通常会通过社交平台、短信或电话，声称“动动手指就能轻松赚钱”，诱导受害者下载指定的App或加入刷单群。
                            <br>

                            受害者完成初次刷单后可能会收到小额返利，犯罪分子利用这种“正反馈”逐渐建立信任，随后要求受害者进行大额刷单或支付“解冻费”、“保证金”等，最终在受害者支付高额款项后直接拉黑或失联。
                        </div>

                        <div class="right-warn-content">
                            <div class="right-top-warn-content">
                                <div class="warn-title">
                                    ✅ 特征提示:
                                </div>
                                <ul class="custom-ul">
                                    <li>出现“高额汇报”、“轻松赚钱”等宣传语时务必提高警惕。</li>
                                    <li>正规客服从不通过私人电话或非官方渠道联系用户。</li>
                                    <li>官方客服不会要求下载任何第三方定位应用。</li>
                                </ul>
                            </div>
                            <div class="right-bottom-warn-content">
                                <div class="warn-title">
                                    🚨 安全建议:
                                </div>
                                遇到类似情况，建议立刻挂断电话，切勿透露个人信息。
                                如已涉及财产损失，请第一时间联系银行冻结账户并报警。
                                下载国家反诈中心App，实时接收反诈预警和最新诈骗动态。
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import NERTC from 'nertc-web-sdk';
// import * as echarts from 'echarts';

export default {
    name: 'VideoChatJoin',
    data() {
        return {
            posts: [
                {
                    avatar: "/src/assets/img/avatar_1.jpg",
                    nickname: '云淡风清',
                    content: '发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态',
                    timestamp: '2025-2-05 8:32'
                },
                {
                    avatar: "/src/assets/img/avatar_2.jpeg",
                    nickname: '(*/ω＼*)暖青',
                    content: '发布了第二条动态',
                    timestamp: '2025-3-02 11:29'
                },
                {
                    avatar: "/src/assets/img/avatar_3.jpg",
                    nickname: '用户A',
                    content: '发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态发布了第一条动态',
                    timestamp: '2025-2-05 8:32'
                },
                // 更多动态...
            ],
            newPost: '',
            questions: [
                {
                    question: '什么是网络诈骗？',
                    options: ['A. 合法的网络推广活动', 'B. 非法获取他人财物的行为', 'C. 在线教育服务', 'D. 网络购物'],
                    explanation: '网络诈骗是指非法获取他人财物的行为。'
                },
                {
                    question: '如何防范网络诈骗？',
                    options: ['A. 不轻信陌生人的信息', 'B. 随意透露个人信息', 'C. 点击所有链接', 'D. 下载所有附件'],
                    explanation: '防范网络诈骗的关键是不轻信陌生人的信息，不随意透露个人信息。'
                },
                // 更多问题...
            ],
            currentQuestionIndex: 0,
            selectedOption: '',
            showExplanation: false,
        };
    },

    computed: {
        currentQuestion() {
            return this.questions[this.currentQuestionIndex];
        }
    },

    watch: {

    },
    mounted() {
        if (!NERTC.checkSystemRequirements()) {
            this.isSupport = false;
        }
    },
    methods: {
        addPost() {
            if (this.newPost.trim() !== '') {
                const newPost = {
                    avatar: "/src/assets/img/avatar_1.jpg", // 默认头像
                    nickname: '你', // 默认昵称
                    content: this.newPost,
                    timestamp: new Date().toLocaleString() // 当前时间
                };
                this.posts.push(newPost);
                this.newPost = ''; // 清空输入框
            }
        },
        nextQuestion() {
            this.currentQuestionIndex = (this.currentQuestionIndex + 1) % this.questions.length;
            this.selectedOption = '';
            this.showExplanation = false;
        }
    },
};
</script>

<style scoped>
.join {
    height: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
}

.content-container {
    width: 80%;
    height: 80%;
    padding: 40px 20px;
    margin-left: 4%;
    display: flex;
    justify-content: flex;
}

.left-content {
    width: 50%;
    text-align: center;
    background: #fff;
    box-shadow: 10px 10px 14px 6px rgba(47, 56, 111, 0.2);
    margin-right: 60px;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
}

.right-content {
    width: 50%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.notes {
    width: 110%;
    background: #fff;
    box-shadow: 10px 10px 14px 6px rgba(47, 56, 111, 0.2);
    border-radius: 8px;
}

.title-bar {
    background-color: #116FCD;
    color: #fff;
    height: 50px;
    font-size: 25px;
    letter-spacing: 1.5px;
    font-weight: bold;
    border-top-left-radius: 8px;
    border-top-right-radius: 8px;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    padding-left: 1vw;
    padding-right: 0.5vw;
}

/* 新增的样式 */
.divider {
    margin-top: 2vw;
    margin-bottom: 2vw;
    border-top: 1px solid #ccc;
}

.post-container {
    padding-left: 8px;
    padding-right: 8px;
    height: 80%;
    /* 设置一个固定高度 */
    overflow-y: auto;
    /* 允许垂直滚动 */
    overflow-x: hidden;
}

.post {
    margin-bottom: 15px;
    padding: 10px;
    width: 96%;
    background-color: #f9f9f9;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    /* 确保内容从左侧开始 */
}

.post-header {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.post-avatar {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin-right: 10px;
}

.post-info {
    display: flex;
    flex-direction: column;
}

.post-nickname {
    font-size: 16px;
    font-weight: bold;
    margin-bottom: 5px;
}

.post-timestamp {
    font-size: 12px;
    color: #888;
}

.post-content {
    margin-bottom: 10px;
    font-size: 16px;
    word-wrap: break-word;
    /* 确保内容可以自动换行 */
    overflow-wrap: break-word;
    /* 确保内容可以自动换行 */
    text-align: left;
    /* 确保内容左对齐 */
    margin-top: 10px;
    margin-bottom: 15px;
}

.post-actions {
    display: flex;
    justify-content: flex-start;
}

.action-button {
    padding: 5px 10px;
    margin-right: 10px;
    background-color: #116FCD;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 12px;
    display: flex;
    /* 添加 */
    align-items: center;
    /* 添加 */
    justify-content: center;
    /* 添加 */
}

.action-button:hover {
    background-color: #0d59b5;
}

.like-button {
    background-color: #c43d60;
}

.like-button:hover {
    background-color: #e6557c;
}

.comment-button {
    background-color: #414656;
}

.comment-button:hover {
    background-color: #6c748f;
}

.input-container {
    display: flex;
    width: 100%;
    justify-content: space-around;
    align-items: center;
    margin-top: 10px;
}

.input-container input {
    width: 70%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.input-container button {
    width: 15%;
    padding: 10px;
    background-color: #116FCD;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.input-container button:hover {
    background-color: #0d59b5;
}

.notes-first {
    display: flex;
    flex-direction: column;
    height: 45%;
    /* justify-content: space-around; */
}

.quiz-container {
    display: flex;
    flex-direction: row;
    height: 70%;
    /* margin-bottom: 10px; */
    padding-left: 18px;
    padding-right: 18px;
}

.quiz-content {
    width: 50%;
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    line-height: 1.5;
}

.question {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.options {
    display: flex;
    flex-direction: column;
}

.option {
    margin-bottom: 5px;
}

.option input[type="radio"] {
    margin-right: 10px;
}

.quiz-explanation {
    width: 50%;
    display: flex;
    flex-direction: column;
    /* margin-top: 20px; */
}

.explanation-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.explanation-content {
    font-size: 16px;
    color: #555;
}

.quiz-actions {
    height: 10%;
    display: flex;
    justify-content: space-around;
    margin-bottom: 20px;
}

.view-explanation,
.next-question {
    padding: 10px 20px;
    background-color: #116FCD;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
}

.view-explanation:hover,
.next-question:hover {
    background-color: #0d59b5;
}

.notes-second {
    height: 50%;
}

.warn-container {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.left-warn-content {
    width: 47%;
    height: 100%;
    font-size: 16px;
    line-height: 1.5;
    letter-spacing: 1px;
    text-indent: 2em;
    margin-right: 10px;
    /* 添加首行缩进 */
}

.right-warn-content {
    width: 47%;
    height: 100%;
}

.right-top-warn-content {
    width: 100%;
    height: 45%;
    font-size: 15px;
    line-height: 1.4;
}

.right-bottom-warn-content {
    width: 100%;
    height: 45%;
    font-size: 15px;
    line-height: 1.4;
    letter-spacing: 1px;
}

.warn-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 6px;
    text-align: center;
}

.custom-ul{
    padding-left:23px;
}

</style>