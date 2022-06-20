<template>
<!-- Modal -->
<div id="edit-form" class="modal-mask">
    <div class="modal-dialog my-modal">
        <div class="modal-content">
            <div class="modal-header bg-primary">
                <h5 class="modal-title text-white">Edit User</h5>
                <button type="button" class="btn-close" v-on:click="hideModal"></button>
            </div>
            <div class="modal-body">
                <form id="add-user-form">
                    <input type="hidden" name="user_id" id="user_id" v-model="selectedUser.id">
                    <div class="mb-3">
                        <label for="name" class="form-label">User Name</label>
                        <input type="text" class="form-control" id="name" name="name" v-model="selectedUser.name">
                    </div>
                    <div class="mb-3">
                        <label for="emails" class="form-label">Email Address</label>
                        <input type="email" class="form-control mb-2" id="emails[]" name="emails[]" v-model="selectedUser.emails[0]">
                        <input type="email" class="form-control" id="emails[]" name="emails[]" v-model="selectedUser.emails[1]">
                    </div>
                    <div class="mb-3">
                        <label for="phones" class="form-label">Phones</label>
                        <input type="text" class="form-control mb-2" id="phones[]" name="phones[]" v-model="selectedUser.phones[0]">
                        <input type="text" class="form-control" id="phones[]" name="phones[]" v-model="selectedUser.phones[1]">
                    </div>
                    
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary"  v-on:click="hideModal">Close</button>
                        <button type="submit" class="btn btn-primary"  v-on:click.prevent="submitForm">Update</button>
                    </div>
                </form>
            </div>
            
        </div>
    </div>
</div>
</template>

<script>
export default({
    name: 'EditForm',
    props: {
        showedit: {
            type: Boolean
        },
        selectedUser: {}
    },
    methods: {
        submitForm(){
            fetch('http://localhost:8000/users',{
                method: 'PUT',
                header: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify(this.selectedUser)
            })
            .then(response => response.json())
            .then(json => {
                console.log(json);
                this.hideModal();
            })
            .catch(err => {
                console.log(err);
            })
        },

        hideModal(){
            this.$emit("modalStatusChanged", false)
        }
    }
})
</script>

<style scoped>
.modal-mask {
    position: fixed;
    top: 0;
    left:0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.4);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 100;
}
.my-modal {
    width: 400px;
    height: auto;
    background-color: #FAFAFA;
    display: flex;
    flex-direction: column;
}
</style>