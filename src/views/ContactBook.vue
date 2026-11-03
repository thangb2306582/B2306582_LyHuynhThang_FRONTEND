<template>
    <div class="page row">
        <div class="col-12">
            <InputSearch v-model="searchText" />
        </div>
        <div class="mt-3 col-md-6">
            <h4>
                Danh bạ 
                <i class="fa-solid fa-address-book"></i>
            </h4>
            <ContactList
                v-if="filteredContactsCount > 0"
                :contacts="filteredContacts"
                v-model:activeIndex="activeIndex"
            />
            <p v-else>Không có liên hệ nào.</p>

            <div class="mt-3 d-flex flex-wrap align-items-center">
                <button class="btn btn-sm btn-primary mr-3" @click="refreshList()">
                    <svg class="action-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" aria-hidden="true">
                        <path d="M13.7 3.3V.8l-1.8 1.8A6.5 6.5 0 0 0 2 7.5h1.6a4.9 4.9 0 0 1 7.2-3.6l-1.8 1.8h4.7V3.3ZM12.4 8.5a4.9 4.9 0 0 1-7.2 3.6L7 10.3H2.3V15l1.8-1.8A6.5 6.5 0 0 0 14 8.5h-1.6Z" fill="currentColor" />
                    </svg>
                    Làm mới
                </button>

                <button class="btn btn-sm btn-success mr-3" @click="goToAddContact">
                    <i class="fa-solid fa-plus"></i> Thêm mới 
                </button>

                <button
                    class="btn btn-sm btn-danger"
                    @click="removeAllContacts"
                >
                    <svg class="action-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" aria-hidden="true">
                        <path d="M5.5 1h5l.7 1H14v1.5H2V2h2.8l.7-1ZM3 4.5h10l-.8 10H3.8L3 4.5Zm3 1.2v7.2h1.3V5.7H6Zm2.7 0v7.2H10V5.7H8.7Z" fill="currentColor" />
                    </svg>
                    Xóa tất cả
                </button>
            </div>
        </div>
        <div class="mt-3 col-md-6">
            <div v-if="activeContact">
                <h4>
                    Chi tiết liên hệ 
                    <i class="fa-solid fa-address-card"></i>
                </h4>
                <ContactCard :contact="activeContact" />
            </div>
        </div>
    </div>
</template>

<script>
import ContactCard from "@/components/ContactCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ContactList from "@/components/ContactList.vue";
import ContactService from "@/services/contact.service";

export default {
    components: {
        ContactCard,
        InputSearch,
        ContactList,
    },
    // Đoạn mã xử lý đầy đủ sẽ trình bày bên dưới
    data() {
        return {
            contacts: [],
            activeIndex: -1,
            searchText: "",
        };
    },
    watch: {
        // Giám sát các thay đổi của biến searchText.
        // Bỏ chọn phần tử đang được chọn trong danh sách.
        searchText() {
            this.activeIndex = -1;
        },
    },
    computed: {
        // Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
        contactStrings() {
            return this.contacts.map((contact) => {
                const { name, email, address, phone } = contact;
                return [name, email, address, phone].join("");
            });
        },
        // Trả về các contact có chứa thông tin cần tìm kiếm.
        filteredContacts() {
            if (!this.searchText) return this.contacts;
            return this.contacts.filter((_contact, index) =>
                this.contactStrings[index].includes(this.searchText)
            );
        },
        activeContact() {
            if (this.activeIndex < 0) return null;
            return this.filteredContacts[this.activeIndex];
        },
        filteredContactsCount() {
            return this.filteredContacts.length;
        },
    },
    methods: {
        async retrieveContacts() {
            try {
                this.contacts = await ContactService.getAll();
            } catch (error) {
                console.log(error);
            }
        },

        refreshList() {
            this.retrieveContacts();
            this.activeIndex = -1;
        },

        async removeAllContacts() {
            if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
                try {
                    await ContactService.deleteAll();
                    this.refreshList();
                } catch (error) {
                    console.log(error);
                }
            }
        },

        goToAddContact() {
            this.$router.push({ name: "contact.add" });
        },
    },
    mounted() {
        this.refreshList();
    },
};
</script>

<style scoped>
.page {
    text-align: left;
    max-width: 750px;
}
.action-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.125em;
}
</style>
