<template>
    <div>
        <ui-edit-area v-model="value"></ui-edit-area>
        {{value}}
    </div>
</template>
<script>
export default {
    data() {
        return {
            value: '我是一个单行文本框'
        }
    }
}
</script>
