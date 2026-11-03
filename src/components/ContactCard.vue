<script>
export default {
    props: {
        contact: { type: Object, required: true },
    },
};
</script>

<template>
<div>
    <div class="p-1">
        <strong>Tên:</strong>
        {{ contact.name }}
    </div>
    <div class="p-1">
        <strong>E-mail:</strong>
        {{ contact.email }}
    </div>
    <div class="p-1">
        <strong>Địa chỉ:</strong>
        {{ contact.address }}
    </div>
    <div class="p-1">
        <strong>Điện thoại:</strong>
        {{ contact.phone }}
    </div>
    <div class="p-1">
        <strong>Liên hệ yêu thích:&nbsp;</strong>
        <svg
            v-if="contact.favorite"
            class="favorite-icon"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 16 16"
            aria-label="Có"
        >
            <path d="M6.2 11.8 2.8 8.4l1.1-1.1 2.3 2.3 5.9-5.9 1.1 1.1-7 7Z" fill="currentColor" />
        </svg>
        <svg
            v-else
            class="favorite-icon"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 16 16"
            aria-label="Không"
        >
            <path d="m4.2 3.1 3.8 3.8 3.8-3.8 1.1 1.1L9.1 8l3.8 3.8-1.1 1.1L8 9.1l-3.8 3.8-1.1-1.1L6.9 8 3.1 4.2l1.1-1.1Z" fill="currentColor" />
        </svg>
    </div>
</div>
</template>

<style scoped>
.favorite-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.125em;
}
</style>
