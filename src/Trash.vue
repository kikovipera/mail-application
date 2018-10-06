<template>

    <div class="inbox-body">
        <app-messages :messages="trashedMessages"></app-messages>
    </div>

</template>

<script>
    import Messages from './Messages.vue';
    export default{
        props: {
            data: {
                type: Object,
                required: true
            }
        },
        computed:{
            trashedMessages: function () {
                return this.data.messages.filter(function (message) {
                    return (message.isDeleted);
                });
            }
        },
        components: {
            appMessages: Messages
        }
    }
</script>