<template>
    <button 
    :aria-label="label"
    :title="label"
    @click="handleClick()"
    class="button-ui">
        <span v-if="buttonText">{{ buttonText }}</span>
        <span v-else>{{ label }}</span>
    </button>
</template>

<script> 
export default {
    name: 'ButtonUI',
    props: {
        label: {
            type: String,
            default: ''
        },

    },

    data() {
        return {
            buttonText: '',
            buttonEvent: this.event,
        };
    },
    mounted() {
        this.buttonText = this.label;
    },
    methods: {
        handleClick() {
            this.$emit('@click', 'jjj');
            console.log(this.$emit('@click'));
        },
    },
    watch: {
        label(newLabel) {
            this.buttonText = newLabel;
        },

    }
};
</script>

<style>
.button-ui {
    font-size: 16px;
    font-family: 'Arial', sans-serif;
    border: 2px solid #a0d4f5;
    border-radius: 10px;
    cursor: pointer;
    background-color: #baebff;
}
.button-ui:hover {
    background-color: #a0d4f5;
    font-size: 18px;
}
.button-ui:active {
    background-color: #8cc3e6;
    font-size: 14px;
}
</style>fkdf