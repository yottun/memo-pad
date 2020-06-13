<template>
    <div class="editor">
        <h1>エディター画面</h1>
        <span>{{ user.displayName }}</span>
        <button @click="logout">ログアウト</button>
        <div class="editorWrapper">
            <div class="memoListWrapper">
                <div class="memoList" v-for="(memo, index) in memos" :key="index" @click="selectMemo(index)" :data-selected="index == selectedIndex">
                    <p class="timer">{{  }}</p>
                    <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
                </div>
                <button class="addMemoBtn" @click="addMemo">メモの追加</button>
                <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">選択中のメモの削除</button>
                <button class="saveMemosBtn" @click="saveMemos">メモの保存</button>
            </div>
            <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
            <div class="previewWrapper">
                <p>{{ current.toLocaleString() }}</p>
                <!-- <div class="preview markdown" v-html="preview()"></div> -->
            </div>
        </div>
    </div>
</template>

<script>
import marked from "marked";
export default {
    name: "editor",
    props: ["user"],
    data(){
        return{
            memos: [
                {
                    markdown: ""
                },
            ],
            selectedIndex: 0,
            current: new Date()
        };
    },
    methods: {
        // ログアウト
        logout: function() {
            firebase
            .auth()
            .signOut();
        },
        // メモ追加
        addMemo: function() {
            this.memos.push({
                markdown: "無題のメモ"
            });
        },
        // メモのセレクト
        selectMemo: function(index) {
            this.selectedIndex = index;
        },
        // プレビュー画面の表示
        preview: function(){
            return marked(this.memos[this.selectedIndex].markdown);
        },
        // メモタイトルの表示
        displayTitle: function(text) {
            return text.split(/\n/)[0];
        },
        // 削除ボタン
        deleteMemo: function() {
            this.memos.splice(this.selectedIndex, 1);
            if (this.selectedIndex > 0) {
                this.selectedIndex--;
            }
        },
        // firebaseへ保存
        saveMemos: function(){
            firebase
            .database()
            .ref("memos/" + this.user.uid)
            .set(this.memos);

            alert("保存しました");
        }
    },
    // 呼び出し
    created: function() {
        firebase
        .database()
        .ref("memos/" + this.user.uid)
        .once("value")
        .then(result => {
            if(result.val()) {
                this.memos = result.val();
            }
        });
        let that = this;
        this.time = setInterval(function(){
            that.current = new Date();
        }, 1000);
    },
    // 保存ショートカットキー
    mounted: function(){
        document.onkeydown = e => {
            if (e.key == "s" && (e.metakey || e.ctrlKey)) {
                this.saveMemos();
                return false;
                alert("保存しました");
            }
        }
    },
    // 無効化
    beforeDestroy: function() {
        document.onkeydown = null;
    }
};
</script>
<style lang="scss" scoped>
.editor{
    max-width: 1000px;
    margin: 0 auto;
}
.editorWrapper {
    display: flex;
}
.memoListWrapper{
    width: 20%;
    border-top: 1px solid #000;
}
.memoList{
    padding: 10px;
    box-sizing: border-box;
    text-align: center;
    border-bottom: 1px solid #000;
    border-left: 1px solid #000;
    &:nth-child(even) {
        background-color: #ccc;
    }
    &[data-selected="true"] {
        background-color: #ccf;
    }
}
.memoTitle{
    height: 1.5em;
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
}
.markdown {
    width: 60%;
    height: 500px;
    padding: 10px;
}
.preview {
    width: 10%;
    text-align: left;
    padding: 0 10px;
}
.previewWrapper {
    width: 20%;
    text-align: left;
    padding: 0 10px;
}
.deleteMemoBtn,
.saveMemosBtn,
.addMemoBtn {
    margin:10px;
}
</style>