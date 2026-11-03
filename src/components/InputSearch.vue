<script>
export default {
    props: {
        modelValue: { type: String, default: "" },
    },
    emits: ["submit", "update:modelValue"],
    methods: {
        updateModelValue(e) {
            this.$emit("update:modelValue", e.target.value);
        },
        submit() {
            this.$emit("submit");
        },
    },
};
</script>

<template>
<div class="input-group">
    <input
        type="text"
        class="form-control"
        placeholder="Nhập thông tin cần tìm"
        :value="modelValue"
        @input="updateModelValue"
        @keyup.enter="submit"
    />
    <div class="input-group-append">
        <button
            class="btn btn-outline-secondary"
            type="button"
            @click="submit"
        >
            <svg
                class="action-icon"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 16 16"
                aria-hidden="true"
            >
                <path d="M11.2 10.2a6 6 0 1 0-1 1l.1.1 3.5 3.5a.7.7 0 0 0 1-1l-3.5-3.5-.1-.1ZM6.5 11a4.5 4.5 0 1 1 0-9 4.5 4.5 0 0 1 0 9Z" fill="currentColor" />
            </svg>
            Tìm kiếm
        </button>
    </div>
</div>
</template>

<style scoped>
.action-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.125em;
}
</style>
