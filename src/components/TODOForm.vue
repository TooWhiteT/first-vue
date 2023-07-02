<template>
    <form @submit.prevent="onSubmit">
        <label for="new-todo-input">
            你需要做什么？
        </label>
        <input type="text" id="new-todo-input" name="new-todo" autocomplete="off" v-model.lazy.trim="label"/>
        <button type="submit" class="btn btn__primary btn__lg">
            添加
        </button>
    </form>
</template>

<script>
export default {
    name: "TODOForm",
    methods: {
        onSubmit() {
            if (this.label === "") {
                alert("创建的待办事项不能为空~")
                return;
            }
            // 事件发布
            this.$emit("todo-added", this.label)
            this.label = ""
        }
    },
    data() {
        return {
            label: ""
        }
    }
};
</script>